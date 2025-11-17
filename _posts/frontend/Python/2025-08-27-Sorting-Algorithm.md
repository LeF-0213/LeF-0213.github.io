---
layout: post
related_posts:
    - /frontend/python
title:  "정렬 알고리즘"
date:   2025-08-27
categories:
  - frontend
  - python
description: >
  
---
* toc
{:toc .large-only}

# 정렬이란?
정렬 -> 모든 데이터를 나열하는 것을 의미(사용자가 정의한 순서대로)
-> 오른차순/ 내림차순

### 정렬이 필요한 이유
데이터를 쉽게 탐색하기 위해서 사용한다.

## 정렬 알고리즘 이론 정리
### 버블 정렬(Bubble Sort)
#### 개념

> 인접한 두 원소를 비교하여 정렬하는 가장 간단한 알고리즘
> 큰 값이 물거품처럼 뒤로 이동하는 모습에서 이름이 유래됨

#### 작동 방식
![bubble_sorting](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FTP0ub%2FbtrYrob3DhR%2FAAAAAAAAAAAAAAAAAAAAAEvEps2l_4XnTODdvDlNjWa-tG-Y8ZyF62AUlgzQBORV%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1764514799%26allow_ip%3D%26allow_referer%3D%26signature%3DxC%252BqDma9dIJic8%252BeNwL5UH3I2Ew%253D)

> 1. 첫 번째 원소부터 인접한 원소와 비교
> 2. 순서가 잘못되어 있으면 교환
> 3. 한 번의 순회가 끝나면 가장 큰 값이 마지막에 위치
> 4. 이 과정을 배열 크기만큼 반복

#### 시간복잡도

| 항목 | 내용 |
|:----:|---------------|
| **시간복잡도(최선)** | `O(n)` - 이미 정렬된 경우 |
| **시간복잡도(평균)** | `O(n^2)` |
| **시간복잡도(최악)** | `O(n^2)` |
| **공간복잡도** | `O(1) (제자리 정렬, in-place)` |

> 정리하면,         
> **회전**은 **배열크기 - 1(n - 1)**번 수행하고,            
> **비교연산**은 **배열크기 - 라운드 횟수(n)**번 수행한다.

#### 장단점

| 장점 | 단점 |
|:---------:|:----------:|
| 구현이 매우 간단 | 속도가 매우 느림
| 추가 메모리가 필요없음(in-place 정렬)| 대량의 데이터에는 비효울적 |
| 안정 정렬 (Stable Srot) | |



## 삽입 정렬(Insertion Sort) 
데이터의 전체 영역에서 정렬된 영역과 정렬되지 않은 영역으로 나눈다.     
데이터가 정렬되어 있을 때는 최고의  발휘한다.
![InsertionSort](https://miro.medium.com/v2/resize:fit:1326/format:webp/1*GzjS6_EJkOcHkdwJdzS8oQ.png)

## 병합 정렬(분할 정렬)
정렬되지 않은 영역을 쪼개서 각각의 영역을 정렬하고 이를 다시 합치는 과정을 하면서 정렬한다.     
병합 과정을 거치기 때문에 추가적으로 메모리가 필요하다.(100% 시간 초과에 걸린다.) 
![MergeSortImg](https://velog.velcdn.com/images/gawgjiug/post/b5c57bc7-9517-4d9c-aa96-6db8cc1ccdb5/image.png)
키 값에 의해서 움직인다.
![MergeSort](https://velog.velcdn.com/images/gawgjiug/post/3218bbfa-7798-4645-be70-a2e483aece69/image.png)

## 위상정렬(Topology Sort)
진행하는 순서에 따라 정렬하는 방식
작업의 순서가 존재할 때 사용되는 알고리즘(진입 차수를 이용)
![TopologySort](https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F8ffd7323-fd31-4969-a052-3463ea4abe40%2Fimage.png)

### 진입 차수와 진출 차수
* **진입 차수(Indegree)**: 특정한 노드로 들어오는 간선의 개수
* **진출 차수(Outdegree)**: 특정한 노드에서 나가는 간선의 개수
![Indegree&Outdegree](https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2Fb78282f8-5082-4221-99cb-853383ad66ec%2Fimage.png)
위상 정렬은 자신을 향한 화살표의 개수를 진입차수로 정의하여 진행

진입 차수가 0이라고 하면 자신을 향한 화살표가 (X)
이 의미는 선행 작업이 필요 없다.

![TopologySort1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbNIvtI%2FbtsHbs2o1NQ%2FAAAAAAAAAAAAAAAAAAAAAEjvKF5unz3WURsvRpkr2y5NXpIcqD6UK7vO0Wz0n_CJ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DyfQkxd4mMC5BZ7W%252FT%252BvEU3UJxrU%253D)

![TopologySort2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FrSlwE%2FbtsHaWQxjoh%2FAAAAAAAAAAAAAAAAAAAAAMXqW8KsGHnVohcTneMO98PKIgiuqeM2RaxJ-3hzED1p%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1756652399%26allow_ip%3D%26allow_referer%3D%26signature%3DzLEl%252BLfCZbUdvjRMEey%252BjrBT7nY%253D)

![TopologySort3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FsXtfx%2FbtsHbeDlL0v%2FAAAAAAAAAAAAAAAAAAAAAIXjMXE_f03P1p9tM5ry0jzj5BGv-wSoCv2rXl4ws25J%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1756652399%26allow_ip%3D%26allow_referer%3D%26signature%3DoIR6MlSNuO7FMMe3Led9YEQkVe4%253D)

![TopologySort4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbeG1ln%2FbtsHd8aiRYn%2FAAAAAAAAAAAAAAAAAAAAABOfp0yKMnkyszWkT6tiDeQP37daVGgJatG2LgWtfiDg%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1756652399%26allow_ip%3D%26allow_referer%3D%26signature%3D8nt0Rfrf5HkHx48MfmuGacONHDc%253D)

![TopologySort5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbvwhzi%2FbtsHa0L8sTi%2FAAAAAAAAAAAAAAAAAAAAAHH4SV5F3rNMSau3Aj6Qh-k0wm6JHTJAxPXv9BG2qDlG%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1756652399%26allow_ip%3D%26allow_referer%3D%26signature%3D9w%252B22%252BfIrbu%252B6Me6w24prhcBM7I%253D)

![TopologySort6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbh2MyI%2FbtsHdvwOhme%2FAAAAAAAAAAAAAAAAAAAAAMl4YuokvLILNadkUxDgB7rBnwdPbm6ISpeq-YMdN3kM%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3D2JJyVRgXP6viq2Xz7J%252F0MCYpeiA%253D)

## 계수 정렬(Counting Sort)
데이터의 빈도의 수를 세어 빈도수 배열에 저장(데이터에 의존하는 정렬 방법)
* **특징**: 시간복잡도
* **딘점**: 데이터에 의존적임으로 항상 사용할 수 있는 기능은 아니다.


![CountingSort1](https://velog.velcdn.com/images%2Fluvlik207%2Fpost%2F0f5454ab-b88d-4e1f-8a49-80b5d88e6f30%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-24%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.03.27.png)

![CountingSort2](https://velog.velcdn.com/images%2Fluvlik207%2Fpost%2Fc7b1d9e9-c8ad-4f95-a786-b378c36c5f38%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-24%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.05.11.png)

문제1]
정렬된 두 배열 arr1, arr2가 존재한다.
이 두 배열을 병합하여 하나의 정렬로 나타내는 코드 작성(오름 차순)

예를 들어 arr1 = [1, 3, 5] arr2 = [2, 4, 6]
=> [1, 2, 3, 4, 5, 6]

```python
arr1 = [1, 3, 4]
arr2 = [2, 5, 6]

def solution(arr1, arr2):
    answer = []

    i = 0
    j = 0

    while (i != len(arr1) and j != len(arr2)):
        if(arr1[i] < arr2[j]):
            answer.append(arr1[i])
            i += 1
            print("if구문 i", i)
        else:
            answer.append(arr2[j])
            j += 1
            print("if구문 j", j)

    while i < len(arr1):
        answer.append(arr1[i])
        i += 1

    while j < len(arr2):
        answer.append(arr2[j])
        j += 1

    return answer
    

print(solution(arr1, arr2))
```

문제2] 정수 n을 받아서 내림차순으로 정렬한
새로운 정수를 반환하는 코드를 작성하시오
n = 1187567
result = 8776511
```python
n = 1187567

def solution(n):
    answer = ""
    dig = list(str(n))
    dig.sort(reverse = True)
    answer = int("".join(dig))
    
    return answer

print(solution(n))
```

문제3] 배열 arr의 i번째 숫자부터 j번째 숫자까지 자르고 
정렬했을 때 k번째에 있는 숫자 구하는 코드를 작성

arr = [1, 3, 7, 2, 4, 6, 8]
i = 2, j = 5, k = 3

3, 7, 2, 4를 뽑아내고 정렬 2, 3, 4, 7
-> 결과는 4
```python
arr = [1, 3, 7, 2, 4, 6, 8]
i = 2
j = 5
k = 3

def solution(arr, i, j, k):
    ar = []
    for x in range(len(arr)):
        if(x >= i - 1 and x < j):
            ar.append(arr[x])
    ar.sort()
    answer = ar[k - 1]
    return answer

print(solution(arr, i, j, k))
```

문제] 정렬이 완료된 두 배열 arr1, arr2를 받아서 병합 정렬하는 함수(solution())를 구하는 코드를 작성하시오
```python
입력 예] arr1 = [1, 3, 5] arr2 = [2, 4, 6] 
```
