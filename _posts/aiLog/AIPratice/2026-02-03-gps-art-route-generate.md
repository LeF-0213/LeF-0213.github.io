---
layout: post
related_posts:
  - /aiLog/AIPractice
title:  "GPS 아트 경로 생성 트러블 슈팅 - Week 2 (2/3~2/4)"
date:   2026-02-03
categories:
  - aiPratice
description: >
    🏁 개발 목표: 사용자가 그린 SVG 자유곡선을 지도 데이터(OSMnx)와 매칭하여, 실제 걸을 수 있는 산책로로 변환하는 알고리즘을 개발
---
* toc
{:toc .large-only}

## 2월 3일 - 프론트 엔드 통합 작업

### 해야 할 일
- 백 프론트 연결
- 속도 최적화
- 잡 경로 없애기

**프론트 연결 과정에서의 설계 결정**

프론트 연결하는 과정에서 경로를 생성하는 API를 받는 `generating.tsx`가 산책 경로, 러닝 경로, 그림 경로 생성 모두 겹치는 부분인 것을 확인했다.

**선택지**:
1. 파일을 분리해서 처리
2. 통합된 파일로 mode 별로 처리
   
**결정**:
파일을 분리해서 해도 되지만, 공통된 부분을 굳이 분리할 필요성을 없을 것 같았다. DB에 각각의 경로에 대한 mode 컬럼을 받기 때문에 **mode마다 다르게 API와 컬럼들을 받아오게 통합할 필요성을 느낀다.**

## 2월 4일 - 타임아웃 문제 해결 (비동기 + 폴링 방식 도입)

### 문제 발생

프론트와 백을 연결하고 개발환경에서 실행해 보았지만, timeout error가 발생했다.

#### 왜 한 번에 요청하면 타임아웃이 날까?

- 클라이언트가 한 번 `POST / generate-gps-art`를 보내고 60초 이상 응답을 기다림
- 이 한 연결이 60초(또는 플랫폼 기본값)를 넘기면 "Network request timed out" 으로 끊김
- 그래서 "한 요청이 너무 오래 걸린다"가 문제

### 해결 방안

여기저기 검색하다보니 **비동기 방식으로 작업하고 + 폴링하는 방식** 이 타임아웃을 피할 수 있을 것 같았다.

#### 비동기 + 폴링이 하는 일

"한 번에 70초 걸리는 요청"을 없애고, "몇 초 걸리는 요청을 여러 번" 보내는 방식이다.

1. **클라이언트: "경로 생성 해줘" (POST)**
  - `POST /generate-gps-art-async` (또는 비동기용 엔드포인트)
  - 바로 (몇 초 안에) 응답: `{ task_id: "abc-123" }`
  - 이때 실제 경로 생성은 아직 진행 중이어도 됨
  - **한 요청이 오래 걸리지 않으므로 타임아웃 안 남**
2. **백엔드: 작업만 등록하고 바로 응답**
  - DB에 "이 사용자의 경로 생성 작업" 한 줄 생성 (`status: processing`)
  - 백그라운드에서 시간이 걸리는 `generate_routes` 실행하여 경로를 생성
  - 클라이언트는 기다리지 않고 `task_id`만 받고 끝
3. **클라이언트: "아직 돼? 완료 됐어?" (GET, 여러번)**
  - `GET /generate/{task_id}`를 2~3초마다 호출(폴링)
  - 한 번 호출은 짧은 요청 (상태만 조회) -> **타임 아웃 없음**
  - 응답 예:
    - `status: "processing", progress: 30` -> 아직 진행 중 -> 2초 뒤 다시 GET
    - `status: "completed", route_id, option_ids`-> 완료 -> 이때만 route-preview로 이동
4. **백엔드: 작업이 끝나면 DB만 갱신**
- 백그라운드에서 경로 생성이 끝나면 해당 `task_id` 행의 `status`, `route_id`, `option_ids` 등만 업데이트
- 폴링하는 GET은 그냥 DB만 읽어서 내려주면 됨

#### 왜 Network request timed out Error를 피할 수 있을까?

**기존**:
- "한 요청 = 70초 대기" -> 그 한 연결이 타임아웃에 걸림

**비동기 + 폴링**:
- "한 요청 = task_id 받기 (몇 초)" -> **타임아웃 없음**
- "그 다음 요청들 = 상태 조회만 (각각 몇 초)" -> **타임아웃 없음**
- 70초 걸리는 일은 백엔드가 백그라운드에서만 하고, 클라이언트는 짧은 요청만 반복해서 결과를 받음

> 즉,     
> 한 번에 오래 걸리는 연결을 아예 만들지 않기 때문에,     
> 플랫폼 타임아웃에 걸리지 않게 되는 것이다.

### DB 스키마 수정

그 과정에서 진행률을 관리하는 DB인 `route_generation_tasks` 중 바뀐 부분을 수정하고, 연결하는 과정을 동시에 진행하고자 한다. 그래야 중간에 processing을 응답을 보내는 코드를 작성할 수 있다.
(현재는 db가 맞이 않아 에러)

#### 컬럼명 수정:
- 컬럼명이 DB와 맞지 않는 부분이 많아, `svg_url` -> `svg_path`로 변경
- `svg_path` 중 routes에 속하는 것은 `custom_svg_path`로 변경

> 그랬더니 500에러가 해결됐다.

## 2월 6일 - 비동기 폴링 방식 완성 및 DB 연동

### 문제점

백에서 `svg_path`와 `custom_svg_path`가 잘못 혼용되어 쓰이고 있어,          
Network 시간 초과 에러 발생 및 `route_generation_task` 진행률을 담당하는 DB의 status가 failed로 찍히고 있었다.

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/857e90eb-6946-46cc-91b0-ec10d250a554" />

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/ab47fbaa-3f48-4119-a8c8-6f0cb0d2fa08" />

> db에 맞게 수정해주니 processing -> completed로 잘 찍히게 되었고, 또한 비동기 + 폴링 방식으로 네트워크 시간 초과 에러가 해결되었다.

### 해야 할 일 - 카카오맵 연동

이제 DB뿐만 아니라 실제 Kakao Map 위에서도 불러올 수 있게끔 프론트를 수정해야 한다. (현재는 예시를 보여주고 있음)

#### 구현 계획

1. 사용자가 경로 생성 완료(`generating.tsx`) 후 `routeId`(및 `optionIds`)와 함께 `route-preview.tsx`로 진입
2. `routeId` 있으면 `getRouteOptions(routeId)` 호출 `optionsLoading` true로 변경
3. 응답 오면 `fetchedOptions`에 출발지 좌표 포함 옵션 3개 저장, `selectedRoute`(사용자에게 우선적으로 보여줄 경로)를 top1으로 설정.
4. `routeOptions = fetchedOptions ?? fallbackOptions` 이므로 리스트는 실제 옵션 3개가 들어감.
5. `mapPolyline = selectedRoute.coordinates`로 채워지고, `mapCenter`는 경로 전체의 기하학적 중심(무게 중심), 즉 인덱스 중간이 아니라 모든 좌표 평균으로 계산한다.
6. KakaoMap에 `polyline={mapPolyline}`, `center={mapCenter}` 전달한다.
  - polyline이 비어있다가 채워지면 `key` 변경으로 KakaoMap이 remount되어, 새 HTML로 **생성된 그림 경로**가 지도에 그려짐.
7. 사용자가 다른 옵션을 택해도, 그 옵션은 `fetchedOptions` 안의 항목이므로 `coordinates`가 있어 경로가 유지된다.

#### 정리

진행률 관련된 것은 `workout.tsx`에만 두고, `route-preview.tsx`는 "옵션 로딩 + 지도에 생성 경로 표시"에만 쓰이도록 위 두 수정으로 정리할 예정이다.

#### 추가 - 진행률 표시 개선 필요

프론트에서 진행률을 보여주는 예시도 실제 진행률과 연결시켜야 한다.

> 연결 시켜보니 폴링 방식을 사용하고 있음에도 중간 단계를 업데이트 하지 않고 10% -> 100%로 가는 구조를 취하고 있어서 중간 단계도 보여줄 수 있게 코드를 추가해야할 것 같다.

## 2월 8일 - 진행률 표시 다중 ID 문제 해결

### 문제점

진행률을 백에 연결에 비동기 폴링 방식으로 경로 생성 중을 자연스럽게 표현하고 싶었으나 문제가 생겼다.

**기존의 10% -> 100%로 바로 넘어가는 부분을 자연스러운 진행률 표기를 위해 업데이트하는 방식을 고치는 과정에서**:

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/d2ac7fd0-28ca-4b5a-bd17-014e4067b857" />

한 가지 task에 대해 **여러 ID가 생기면서** 진행률이 올라가고 있다. 때문에 프론트에서 보여주는 것도 첫 시작으로 지정했던 5%에 머물고 있고, 실제 변경된 Progress는 받아지지 않고 있어 경로 생성 중에만 화면이 멈추게 되었다.

### 원인 분석

보니까 POST가 **useEffect()안에서 돌면서 여러 번 나가는 현상**이 발생한 것이다.

### 해결 방법

**useEffect 안에서 값이 변해도 리렌더링 발생하지 않는 `useRef`를 사용** 해서:
- 이미 한 번 시작했는지 표시해 두고
- 두 번째 실행부터는 `run()`/POST를 하지 않도록 하는 방식으로 코드를 수정해야겠다.

### 결과

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/df96182b-7c2b-4ef4-a70b-7d121bbcd7e6" />

ID가 여러 개 생기지 않고 한 ID만으로 100%까지 진행되며 잘 완료되었다.

## 2월 9일 - 유사도 개선 및 속도 최적화 시작

### 해야 할 일

1. **속도 최적화**
  - 유사도 함수 벡터화(이중/삼중 for문을 numpy로 한 번에 거리 행렬 계산하도록 바꾼다.)
  - k배치마다 angle을 다르게 한 것을 병렬 처리(품질은 동일, 조합 수는 그대로 두고 wall-clock만 감소)
2. **불필요한 경로 없애기**
  - 진행 방향 따라가는 함수가 효과가 없음. (지우든지 가중치를 수정하던지 해야 함), 속도에는 별 차이 없어서 일단 그대로 둠
  - 생성된 경로 top3가 선정되면, 그때 연결되지 않고 뻗어나간 불필요한 경로를 삭제하는 방향으로 생각 중

### 문제점

<div style="display: flex; gap: 10px">

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/bca734ef-1958-4ad9-a9c0-3f9d861598e9" />

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/7eeeac41-f343-462d-95ac-8303929f44c8" />

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/229fd2d4-a961-47e9-be45-2bb95513eef3" />

</div>

### 문제 - 유사도 순위 불일치

유사도 벡터화를 진행하기 전, **유사도 점수가 실제 보이는 것과 순위가 다르게 적용** 되고 있었다.

- 현재 유사도 점수가 낮을수록 유사한 것이라 오름차순 정렬 후 `top3 = all_candidates[:3]` 를 이용했으나, 지표와 체감이 다른 것 같다.

### 원인 - 마지막 세그먼트 누락

루프 안에서 `s = original_drawing[i]`, `e = original_drawing[i + 1]`만 쓰는데, 마지막 세그먼트가 중복될 것을 우려하여 `range(len(original_segments) - 1)` 도 함께 적용하여 **마지막 세그먼트가 아예 적용이 되지 않고 있음**

### 해결 방안

세그먼트 적용 후는 다시 1순위가 가장 유사한 것으로 잘 가져옴

### 문제 - 로테이션 일부 적용

로테이션도 경로 상의 한 점마다 다른 것을 집어넣도록 코드를 작성하였는데, 그것이 잘 적용되지 않고 있다. 이유를 살피려고 해당 코드를 살펴보니 **들여쓰기 실수로 for문 바깥에서 마지막 로테이션만 적용하고 있었다....**

### 들여쓰기 수정 후

<div style="display: flex; gap: 10px">
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/cc0d9aab-1e94-40b9-8dd1-675cf524f114" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/9f6b1c2e-2ac4-4dbd-8cac-cdc47b16255b" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/2ca02a51-c95b-4e6a-99e3-87e98782daf6" />
</div>

들여쓰기를 수정해서 원래 의도대로 K배치마다 로테이션 스케일 값을 다 다르게 적용했더니:
- 속도가 안 그래도 경로 상의 한 점으로 바꾸는 과정에서 늘어났는데 **더 늘어나게 되었다.**
- 그래도 확실히 **로테이션이 다양하게 적용** 되기는 하는 것 같다.

### 문제 - 품질 저하 원인 분석

**품질 자체가 테스트 코드랑 완전히 같은 코드를 씀에도 전체적으로 품질이 떨어진 이유**를 알아봐야할 것 같다.

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/132741a8-5698-40c4-8af8-4f7dd793d469" />

### 문제 원인

코드는 테스트 때와 완전히 같기 때문에 품질이 떨어진게 아니라, **출발지가 달라지면서 테스트 때와 다른 결과가 나온 것 같다.**

프론트에서 받아온 현재 위치를 표현해주는 `S 마커` 위치가 지정해준 것과 다르게 받아진다. 현재 위치를 받는게 아니라 **`linePath[0]` 이라고 경로 첫 좌표를 가졍고 있었다.**

<div style="display: flex; gap: 10px">
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/006ae489-9243-4cd8-bf50-d91784d4de8d" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/7abe5488-52e7-4b42-87ae-f18b86ebc851" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/d53498a8-29c5-48ba-8f6f-6dbb3822662f" />
</div>

1. **S마커 위치 수정**
  - `linePath[0]`이 아니라 db에 저장된 출발지로 `S마커`가 표시되게 끔 잘 바뀌었다.
1. **양방향 유사도 계산**
  - 유사도 측정 함수를 numpy를 이용해 행렬 연산을 하니 속도도 빨라졌다.

### 유사도 함수 로직 변경 - 양방향 유사도 계산

<div style="display: flex; gap: 10px">
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/f3818554-785c-4e1f-bd09-4f84c218eac0" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/5939904f-00b6-41d6-9074-fea98b1ad170" />
</div>

- 여전히 품질이 별로인 것 같아, 유사도 함수를 `그림 -> 경로`, `경로 -> 그림`의 차이를 둘 다 계산하는 식으로 바꾸었더니 원본과의 경로도 더 품질이 향상되었다.
- 테스트 할 때도 품질이 향상된 것을 볼 수 있다.

### 새로운 문제점 발생

다만, 한가지 문제점이 또 생겼는데, **K배치(그림경로 상의 한 점)마다 로테이션을 하는 형식이라 그림이 완전히 겹쳐버릴 때도 있었다.**

<br />

---

## 2월 10일 - 대규모 성능 최적화

### 속도 최적화 계획
- **(K, angle) 조합을 병렬 처리** (품질 동일, 조합 수 그대로 두고 wall-clock만 감소)
- **세그먼트 캐시** (현재 매번 K배치마다 계산하기 위해 매번 start_idx를 바꾸고 있음, 그래서 같은 경로를 다시 생성하게 되기도 함)
- 양방향 A*(시작↔︎목표 양쪽에서 동시에 찾아 A*1회당 시간이 줄어듦)
- 워커별 A*캐시

### (K, angle) 조합 병렬 처리

#### 조합이 어떻게 만들어지는지
1. **K(배치 인덱스)**:
  - 그림 폴리라인을 균등하게 나눈 "배치 인덱스"
  - `n_placements = 30`이면 `k in range(30)` -> 30가지
2. **angle(회전 각도)**:
  - 도형을 회전시키는 각도
  - 기본이 `range(-180, 180, 10)`이면 -> 36가지
3. **조합 수**:
  - 30 x 36 = 1,080개의 (k, angle) 조합
  - 각 조합마다 "이 k에서 시작하고, 이 angle로 회전한 도형"에 대해 경로 후보 하나를 만듦
  - **조합 수를 바꾸지 않으면 품질은 그대로다.**

#### 한 조합당 실제로 하는 일 

1. **좌표 변환**: translate -> rotate -> scale (해당 k, anlge 기준)
2. **웨이포인트 계산**: 스케일된 도형 위에 노드들을 올림
3. **세그먼트 경로**: 웨이포인트 사이를 그래프 위에서 A* 으로 연결
4. **시작점 후보별 루프**: `start_inx in range(len(wp_nodes))`만큼 "시작점을 어디로 할지" 바꿔가며
  - 전체 노드 경로 생성
  - 거리/좌표 계산
  - 유사도 계산
5. 그 (k, angle) 안에서 가장 유사도 좋은 경로 하나를 골라 (angle, scaled, candidate) 형태로 반환

> 죽,         
> - 1,080개 조합 = 1,080번의 독립된 "후보 하나 계산"
> 각각은 위 1 ~ 5 단계를 전부 수행
> 이 1,080를 그대로 유지하는 것이**"조합 수 그대로, 품지 동일"**하게 유지할 수 있다.

#### 병렬 처리로 wall-clock만 줄이는 방식

```python
max_workers = min(os.cpu_count() or 4, len(tasks), 8)

with ProcessPoolExecutor(max_workers=max_workers, 
                        initializer=_init_worker, 
                        initargs=initargs) as executor:
    futures = {executor.submit(_run_one_candidate, t): t for t in tasks}
    for future in as_completed(futures):
        result = future.result()
        ...
```

#### 주요 구성 요소

1. **initializer / initargs:**
  - 각 워커 프로세스가 맨 처음 한 번만 `_init_worker(..., graph, drawing_lonlat, sampled, ...)`를 호출
  - 그래프・도형・sampled 등을 전역 변수로 올려 둠
  - 그래서 `_run_one_candidate`는 (k, angle) 두 숫자만 인자로 받고, 나머지는 워커 내부 전역에서 읽음
2. **executor.submit(_run_one_candidate, t):**
  - 각 (k, angle) 조합 t에 대해 "이 조합만 계산하는 작업"을 하나씩 풀어 제출
  - 제출만 하면 바로 다음 조합 제출로 넘어가고, 실제 계산은 워커들이 동시에 함
3. **as_completed(futures):**
  - "끝난 작업부터" 순서없이 하나씩 꺼내서 `result()`로 결과를 모음
  - 먼저 끝난 워커가 바로 다음 (k, angle) 작업을 받아서 이어서 계산

#### 정리

**품질**:           
- 여전히, 1,080개 (K, angle) 조합 전부에 대해 `_run_one_candidate`를 한 번씩 실행
- 탐색 공간・후보 개수 동일 -> **품질(동일 조합 수) 유지**

**wall-clock**:       
- 동시에 돌아가는 프로세스 수만큼 "동시에 여러 조합"이 처리되므로
- 총 시간 ≈ (한 조합당 평균 시간) ✕ (1,080 / 동시 실행 수) + 오버헤드
- 예: 8 워커면 이론상 순차 대비 약 **1/8 수준**까지 줄어듦(그러나 컴퓨터 성능상 실제는 그렇지 않음)
- 같은 일을 더 짧은 실제 경과 시간에 수항하는 것이 **"wall-clock만 감소"** 했다고 함.

> 즉,       
> **(K, angle) 조합을 병렬 처리한다는 것은**:           
> "조합 수나 로직을 줄이지 않고,       
> 그 1,080개를 여러 CPU 코어에 나눠서 동시에 돌려 전체 소요 시간만 줄인다"는 의미이다.

#### 결과

<div style="display: flex; gap: 10px">
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/df51e500-dc91-4057-b158-bd03a37197af" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/a365729f-d3c2-41cb-acda-7ad1cf399589" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/16006625-2378-4169-9e74-1d1b7e7401ce" />
</div>

> **120초 정도 속도가 줄었다.** 그러나 원하는 생성 시간보다 오래 걸리기에 다른 방식들도 적용해봐야겠다.

### 세그먼트 캐시 도입

#### 현재 상황 분석

**지금은 어떻게 돌아가나?**

1. **ProcessPoolExecutor**로 (k, angle) 태스크를 여러개 돌린다.
2. **각 태스크마다**:
  - `_compute_segment_paths(wp_nodes)`호출
  - waypoint 인접 쌍 (`wp[i], wp[i + 1]`)구간마다 A*를 한 번씩 돌림
3. **같은 (k, angle) 태스크 안에서는**:
  - 이미 세그먼트 캐시로 `(wp[i], wp[i + 1])`당 A* 1번만 하고 있음

#### 문제 - "다른 태스크"끼리는?
**서로 다른 태스크의 waypoint**:
- (k1, angle1) 태스크의 waypoint: `wp_1 = [노드A, 노드B, 노드C, ...]`
- (k2, angle2) 태스크의 waypoint: `wp_2 = [노드X, 노드Y, 노드Z, ...]`

**중복 발생**:
- k/angle이 다르면 대부분 wp도 달라서
- `(노드A, 노드B)` 같은 구간이 서로 다른 태스크에서 반복될 수 있음
- **태스크가 바뀔 때마다** 이전에 계산한 `(노드A, 노드B)` 결과를 기억해 두지 않고 **매번 다시 A*를 돌림**

#### 개선 방안 - 세그먼트 캐시

**핵심 아이디어**:
- 같은 waypoint 순서에서 `start_idx`만 바꿀 때, 실제로 쓰는 엣지 구간 `(wp[i] -> wp[i + 1])`은 항상 30개로 동일하다.
- 그런데 지금은 `start_idx`마다 `build_full_path`를 새로 호출해서 같은 구간에 대해 A*를 매번 반복하고 있다.

**해결 방법**:
- waypoint 리스트에 대해 `(wp[i], wp[i+1])` 구간당 A*를 1번만 돌리고
- 그 결과(세그먼트 경로들)를 캐시한 뒤
- 각 `start_idx`에서는 이 세그먼트들을 **순서만 바꿔서 이어붙이기만** 하면 됨

**효과**:
- (k, angle)당 A***900번 -> 30번**으로 줄어듦
- 해당 부분이 이론상 최대 약 **30배 빨라질 수 있음**
- (전체 395초 중 상당 부분이 이 구간이라, 전체 시간이 크게 줄어들 수 있다.)

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/cefd1cd3-faf7-45ba-a608-7f3997c6cf59" />
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/74857e77-0bc4-4766-ad12-e2b3fa8b23cf" />

> segment 캐시로 중복되는 값을 계산하지 않으니 150초 정도 더 줄어든 것을 볼 수 있다. 테스트시에도 경로가 중복없이 다양하게 나왔다.

### 양방향 A* 적용
#### 왜 양방향인가?

**일반 A*의 한계**:
- start -> goal 한 방향으로만 퍼져 나가서
- 목표에 닿을 때까지 탐색 범위가 커질 수 있음

**양방향 A*의 장점**:
- start에서 나가는 탐색과 goal에서 나가는 탐색을 같이 돌리면서
- 중간에 만나는 노드에서 두 경로를 이어서 최단 경로를 만듦
- **최단 경로 품질은 그대로 두고**, 같은 비용까지 탐색하는데 펼쳐지는 노드 수를 줄여서 보통 더 빨리 끝난다.

#### 두 개의 탐색

| 구분        | Forward (f)                                  | Backward (b)                                   |
|------------|----------------------------------------------|------------------------------------------------|
| 방향       | start → goal                                 | goal → start                                   |
| 프론티어   | frontier_f (우선순위 큐)                     | frontier_b (우선순위 큐)                       |
| 비용       | cost_so_far_f[node] = start ~ node 비용      | cost_so_far_b[node] = goal ~ node 비용         |
| 이전 노드  | came_from_f                                  | came_from_b                                    |
| 휴리스틱   | 현재 노드 → goal 까지 거리                   | 현재 노드 → start 까지 거리                    |

#### 결과

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/d6e89c15-4065-4edc-aa8f-eb54417a8688" />

> 양방향 A* 적용 결과 약 30초가 줄어들었다.

### 워커 간 캐시 공유
#### **어떻게 해야 속도를 더 줄일 수 있을까 하고 알아보니**:
(k, angle) 태스크를 병렬처리로 여러개 돌리고 있는데:

같은 태스크 안에서는 세그먼트 캐시로 A*를 1번만 하고 있다
하지만 다른 태스크끼리는 중복이 제거되지 않아 시간이 더 오래걸릴 수 있음

#### 아이디어:
다른 (k, angle)인데, 우연히 (노드10, 노드17) 같은 같은 (node_a, node_b) 구간이 둘 다 나올 수 있기에 그 중복을 제거한다면 시간을 줄일 수 있을 것 같다.

#### 결과:
그러나 오히려 캐시로 건너뛴 노드쌍은 없어서 그런지 더 오래걸렸다.

### 거리 보정 로직 개선

#### 문제점

거리 보정을 하면서 한 번 더 과정을 더 도는데 **이 과정이 굉장히 오래 걸린다.**

#### 해결 방안

따라서 **기존 후보를 재사용해서 가벼운 2차 패스**로 바꾸려고 한다. 이미 구해놓은 후보들 중에서 유사도가 좋은 순으로 정렬하고 사용자가 원하는 거리에 더 가까운 걸 선택하는 방식으로 바꿔야 겠다.

#### 결과

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/8650a315-f6c2-4414-a35f-5e96f389cf4d" />

> - **약 80초 정도 속도가 줄었다.**
> - 혹시 몰라 품질도 확인해보니, 품질이 떨어지진 않은 것 같다.

### 거리 오차 문제

다만 확실히 경로의 길이 오차가 커져 **거리 오차를 강하게 패널티**를 주어 시간은 그대로되 거리 오차를 줄여봐야겠다.

1. **패널티 강화**
  - 패널티는 별로 효과가 없는 것 같다,
2. **스케일 값 조정**
  - 원본 그림보다 구불구불하게 되서 길어지는 것이라고 생각해, **스케일 값을 줄이는 방향** 으로 바꾸어 보려고 한다.

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/25f3c5ef-d93a-4d6b-af5c-dcbf2c7d92ee" />

> 효과가 있었다. 품질에는 영향이 없고 5km정도로 잘 맞춰진 거 같다.

#### Feedback 함수 제거 검토

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/3860f167-c839-4573-9750-a790c60379d3" />

feedback 함수 자체를 사용하지 않는 방향으로 정해도 될지 제거해보았는데, 사용하지 않아도 경로 보정이 잘 된다.

### 그래프 전처리 및 단순화

RoadNetworkFetcher가 가져오는 그래프에서:
- 차선 병합
- degree-2 체인 압축 같은 단순화 (토폴로지는 유지, 노드 수만 줄이기)

> 이렇게 하면, A*가 탐색해야 하는 노드 수가 줄어서 **같은 경로를 더 빨리 찾을 수** 있을 것 같다.

#### degree-2 체인 압축

**"degree-2 체인 압축"이란**:       
한 줄로 이어진 **"중간 노드"들을 없애는 것**

**예시**

```
압축 전:
A --- B --- C --- D
  (B, C는 degree-2)
  
압축 후:
A -------------- D
```

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/8b1aa5a5-615b-461a-99af-80323ad4dabe" />

#### 차선 병합

같은 구간을 여러 번 표현한 **"평행/중복 엣지들"을 하나로 묶는 것**

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/925f710c-2de2-48c1-b4a9-4a7e7a12c520" />

> - 차선 병합은 보행자 도로라 별로 효과가 없는 것 같다.
> - 실제로 차이가 크지 않고 오히려 초수가 미미하지만 늘어났다.
> 그래서 해당 로직은 사용하지 않기로 했다.

## 2월 11일 - 추가 속도 최적화 노력

### 현재 상황 분석
#### 현재 속도: 알고리즘 상 50초대

그럼 테스트 폴더와 다르게 앱이 더 시간이 오래걸리는 이유는 무엇일까?

#### 가능한 원인들
1. **Progress 콜백 -> DB커밋 반복**
2. **그래프 fetch 환경 차이**
  - 앱은 서버(또는 클라우드)에서 매 요청마다 Overpass API 호출하고 있음
3. **멀티프로세스 경합**
  - 앱: Gunicorn/uvicorn 등으로 여러 워커가 돌면
  - 경로 생성 백그라운드 태스크 + ProcessPoolExecutor 자식 프로세스들이 같은 CPU를 나눠 씀

#### 진행률 콜백 최적화

**현재**:
- 매 작업이 끝날 때마다 진행률 콜백을 하고 있음
**수정**:
- 10번 단위로 수정하니 체감상 속도가 줄어들었다.

























