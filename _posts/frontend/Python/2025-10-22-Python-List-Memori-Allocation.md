---
layout: post
related_posts:
    - /frontend/python
title:  "Python 리스트 메모리 동작 과정"
date:   2025-10-21
categories:
  - frontend
  - python
description: >
  Python 리스트 요소 추가, 삽입, 삭제, 인덱싱 시 메모리 동작 과정 상세 이해
---
* toc
{:toc .large-only}

# Python 리스트의 메모리 동작 과정
## CPython 내부 구조: PyListObject
Python 리스트는 C언어로 구현된 `PyListObject` 구조체이다.
### PyObject_HEAD 매크로
![list](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FrbbGS%2FbtsNlJ3pII6%2FAAAAAAAAAAAAAAAAAAAAADZ_4vWsTlsb7UDjjXvI68BXsS1bijEAnOMx_1PO2yF8%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3D1JlxIGO35BdjtPMnwOSGbGwqy0I%253D)
### PyObject_HEAD
```c
#define PyObject_HEAD \
    Py_ssize_t ob_refcnt;      // 참조 카운트 (가비지 컬렉션용)
    PyTypeObject *ob_type;     // 타입 정보 (list, dict 등)
```
* 모든 Python 객체는 이 두 필드를 반드시 가진다.
* `PyObject_HEAD`는 `C언어`로 구현된 `파이썬 인터프리터(CPython)` 의 `구조체(struct)`에 포함된 매크로이다.
* 파이썬 객체라면 반드시 가져야 하는 공통 필드를 정의 합니다.

### PyListObject 구조체
```c
typedef struct {
    PyObject_HEAD         // 공통 필드: 참조 횟수, 타입 정보 포함!
    Py_ssize_t ob_size;   // 리스트의 길이
    PyObject **ob_item;   // 실제 아이템 배열 포인터
    Py_ssize_t allocated; // 할당된 배열 용량
} PyListObject;
```

## 메모리 구조 상세
### Stack과 Heap의 역할
**Stack (스택)**
  * 변수명 `li`만 저장
  * 실제 값이 아닌 **메모리 주소**만 가짐
  * 예: `li -> 0x1000`

**Heap (힙)**
  * 실제 `PyListObject` 구조체가 저장됨
  * 리스트의 메타데이터와 요소들이 여기에 존재

# 추가 시 메모리 동작(여유 용량이 존재할 때), 용량 초과시 메모리 동작(재할당 발생)
![listMemory1](https://github.com/user-attachments/assets/7e873d81-84b2-4097-b174-995576b8a9f1)
## 요소 추가: `li.append(4)`
### 동작 과정
* 여유 공간 확인: `ob_size (3) < allocated (4)`
* `ob_item[3]`에 `4`를 추가
* `ob_size`를 3 -> 4로 증가
* **배열 재할당 불필요!**

### 메모리 상태 (변경 후)
```Python
Heap (0x1000):
  PyListObject {
    ob_size: 3 -> 4           // 길이만 증가
    allocated: 4              // 용량 변경 없음
    ob_item -> [1, 2, 3, 4]   // 여유 공간에 4 추가
  }
```
### 시간 복잡도
* **O(1)**: 상수 시간
* 이미 할당된 공간에 바로 추가하므로 매우 빠름

## 초기 상태: `li = [1, 2, 3]`일 때
### 메모리 상태
```Python
Stack:
  li -> 0x1000

Heap (0x1000):
  PyListObject {
    ob_refcnt: 1            // 참조 카운트 1개 (li만 가리킴)
    ob_type: &PyList_Type   // list 타입
    ob_size: 3              // 길이 3
    allocated: 4            // 용량 4 (여유 공간 1개)
    ob_item -> [1, 2, 3, (여유)] 
  }
```

### 핵심 포인트
* **ob_size (3)**: 실제로 사용 중인 요소 개수
* **allocated (4)**: 할당된 총 공간 (여유 공간 포함)
* **여유 공간**: `allocated - ob_size = 1`
* 리스트는 **미리 여유 공간을 확보**하여 **효율성**을 높임

## `li.append(5)`전 상황
* 현재: `ob_size = 4`, `allocated = 4` (꽉 참)
* `5`를 추가하려면 공간 부족

## 재할당 과정(Over-allocation)
### 1단계: 용량 계산
**CPython의 리스트 재할당 공식**
```c
new_allocated = (ob_size >> 3) + (ob_size < 9 ? 3 : 6) + ob_size
```
* `ob_size >> 3`
  * **비트 시프트 연산**: 오른쪽으로 3칸 이동
  * **의미**: `ob_size / 8`과 동일
  * **목적**: 현재 크기의 1/8을 추가 여유로 확보
* `(ob_size < 9 ? 3 : 6)`
  * **목적**: 작은 리스트에는 작은 여유, 큰 리스트에는 큰 여유 확보
* `ob_size`
  * 최소한 현재 크기는 보장

#### 예시 - ob_size = 4일 때
```python
new_allocated = (4 >> 3) + (4 < 9 ? 3 : 6) + 4
              = (4 / 8) + 3 + 4
              = 0 + 3 + 4
              = 7
```
`7`이지만 **`8`로 반올림** 한다.(2의 거듭제곱 선호)

#### CPython의 리스트 재할당 공식을 쓰는 이유
**간단한 2배 증가의 문제점**
* 큰 리스트에서는 **메모리 낭비가 심함**
* 1000개 요소인데 2048개 공간을 할당한다면 1048개가 낭비됨

```python
# 작은 리스트: 빠르게 증가 (2배)
4 → 8 → 16

# 큰 리스트: 천천히 증가 (약 1.125배)
16 → 25 → 35 → 46 → 58 ...
```

### 2단계: 새 배열 할당
* 메모리에 더 큰 배열 생성 (크기 8)
* 기존 요소들을 새 배열로 **복사**
* 새 요소 `5` 추가
* 기존 배열 메모리 해제

### 3단계: 메모리 상태 업데이트
```python
Heap (0x1000):
  PyListObject {
    ob_size: 4 -> 5
    allocated: 4 -> 8     # 용량 2배 증가
    ob_item -> [1, 2, 3, 4, 5, (여유), (여유), (여유)]
  }
```

### 시간복잡도
* 재할당 발생 시: **O(n)** (기존 요소 복사)
* 하지만 **분할 상환 분석**으로 평균 **O(1)**
* 재할당이 가끔만 발생하므로 전체적으로는 효율적

# 중간 삽입 시 메모리 동작, 요소 삭제 시 메모리 동작, 인덱스 접근 시 메모리 동작
![listMemory2](https://github.com/user-attachments/assets/f7292361-ba7f-49c0-af0f-4dc6b47c84bc)
## 중간 삽입: `li.insert(1, 10)`
### 동작 과정
* 공간 확보 확인
  * 여유 공간 있으면 재할당 불필요
  * 없으면 위의 재할당 과정 먼저 실행
* 요소 이동
  * 인덱스 `1`에 삽입 -> 인덱스 `1~4`의 요소들을 오른쪽으로 이동
  ```python
  [1, 2, 3, 4, 5]
      ↓
  [1, _, 2, 3, 4, 5]  # 인덱스 1 비움
      ↓
  [1, 10, 2, 3, 4, 5] # 10 삽입
  ```
* 메타데이터 업데이트
  * `ob_size` 증가: **5 -> 6**

### 시간복잡도
* **O(n)**: 이동해야 할 요소 개수에 비례
* 앞쪽에 삽입할수록 느림(더 많은 요소 이동)
* 끝에 삽입(`append`)이 가장 빠름

## 요소 삭제: `li.pop(1)`
### 동작 과정
* 요소 제거
  * 인덱스 `1`의 요소(`10`) 삭제
* 빈 공간 메우기
  ```python
  [1, 10, 2, 3, 4, 5]
      ↓
  [1, _, 2, 3, 4, 5]   # 빈 공간 발생
      ↓
  [1, 2, 3, 4, 5, _]   # 뒤 요소들을 왼쪽으로 이동
  ```
* 메타데이터 업데이트
  * `ob_size` 감소: 6 -> 5
  * `allocated`는 **변경하지 않음** (공간 유지)

### 시간복잡도
* **O(n)**: 이동해야 할 요소 개수에 비례
* 앞쪽 삭제일수록 느림
* 끝에서 삭제(`pop()`)가 가장 빠름: **O(1)**

## 인덱스 접근: `li[2]`
### 동작 과정
* 요소 접근
* 주소 계산
  ```c
  // C 수준에서의 실제 동작
  address = ob_item + (index * sizeof(PyObject*))
  ```
* 직접 계산
  * 계산된 주소로 바로 이동
  * 중간 요소를 거치지 않음

### 시간복잡도
* **O(1)**: 상수 시간
* 배열은 **연속된 메모리 공간**이므로 인덱스만으로 위치 계산 가능

## 핵심 개념 정리
### 동적 배열 (Dynamic Array)
* Python 리스트는 **크기가 변하는 배열**
* 필요에 따라 자동으로 크기 조정

### Over-allocation(여유 공간 확보)
* 항상 실제 필요량보다 **조금 더 많은 공간** 할당
* 매번 재할당하는 비효율 방지
* allocated > ob_size 유지

### 재할당 전략
* 공간 부족시 **약 1.125배 + 상수** 증가
* 작은 리스트: 빠르게 증가
* 큰 리스트: 천천히 증가
* 메모리 낭비와 성능의 균형

### 참조 기반 저장
* 리스트는 **객체의 포인터**만 저장
* 실제 데이터는 Heap의 다른 곳에 존재
* `ob_item`은 포인터 배열

### PyObject_HEAD의 중요성
* **모든 Python 객체**의 공통 필드
* `ob_refcnt`: 가비지 컬렉션 관리
* `ob_type`: 타입 정보 (런타임 타입 체크)