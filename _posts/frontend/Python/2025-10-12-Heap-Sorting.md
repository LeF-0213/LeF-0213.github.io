---
layout: post
related_posts:
    - /studylog/python
title:  "힙 정렬"
date:   2025-08-27
categories:
  - studylog
  - python
  - algorithm
description: >
  
---
* toc
{:toc .large-only}

# Heap 정렬
1. 주어진 데이터로 합을 구축해야 한다.(정렬이 되어있지 않은 데이터로 힙을 구축)
2. 그 다음 힙정렬을 한다.

데어터의 삽입과 동시에 빠르게 정렬할 수 있다.

## Heap이란?
**최솟값** 또는 **최댓값**을 **빠르게** 찾아내기 위해 **완전이진트리** 형태로 만들어진 자료구조이다.
![BinaryTree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FrxtPo%2FbtqZTU0RtIB%2FAAAAAAAAAAAAAAAAAAAAAEa-CfsKNKQgwvwtoNYdYAZeHybI8cksKZHOjdCJK6wk%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DFdyJtn68ITsDnQlx3yg0U7FVP90%253D)

![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FlR7aO%2FbtqZSuVD2vb%2FAAAAAAAAAAAAAAAAAAAAAP4RP2moe9XAe3MSmP4fO98kxAr9SJZxiLWgCrSM97cU%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DuBAXwZEON8THrpfzJZDoaCH%252F%252F%252BA%253D)
* 최대 힙: 부모 노드의 값(key 값) >= 자식 노드의 값(key 값)
* 최소 힙: 부모 노드의 값(key 값) <= 자식 노드의 값(key 값)

![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbaNi4n%2FbtqZ2csFHgz%2FAAAAAAAAAAAAAAAAAAAAABL6CYDeUa-Y5KmmgEwMQDXYxEwHLHG31Tpr_RgFltZ4%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DTGktBDy5FnF7TIcYVkyOIoOanH0%253D)
특정 규칙이 있는 이진 트리, 이 규칙에 따라 최대 힙, 최소 힙으로 나누어진다.
* 최대 힙: 부모 노드가 자식노드보다 크다.
* 최소 힙: 부모노드가 자식노드보다 작다.

힙 구축 방법: 최대 힙
정렬되지 않은 데이터를 통해 최대(최소) 합을 구축
최대 힙 함수 => max.heapify() * 배열의 인덱스로 작동
: 특정 노드가 최대힙의 규칙을 만족하지 못하면 합을 구축하는 과정을 반복하는 동작

1. 현재 노드와 자식 노드 값을 비교
- 현재 노드의 값이 가장 크지 않으면 자식 노드 중에서 가장 큰 값과 현재 노드의 값을 바꾼다.
- 만약 자식 노드가 없거나 현재 노드의 값이 가장 크면 연산을 종료
1. 바꾼 자식 노드의 위치를 현재 노드로 하여 1번 과정을 반복한다.

max.heapify(배열의 인덱스 값)
비교할 자식 노드가 없으면 아무런 동작을 하지 않는다.

시작은 루트와 마지막 노드를 교환하고 시작

힙 정렬 로직은 루트 노드를 제외하고, 힙을 다시 구축
최대 힙에서 루트 노드는 가장 큰 값이므로 루트 노드를 제외하고 다시 힙을 구축했을 때의   
루트 노드가 두번째로 큰 값이 된다는 성질을 이용하여 정렬
![MaxHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbPuVM8%2FbtqZ0tCzTir%2FAAAAAAAAAAAAAAAAAAAAAFIQLWJtPoB3-htB63xIj379GLOgBTkEGGAfLOty-WwG%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DEwQVH2QE%252FXswFJhoWFrVKeggx9w%253D)

![MaxHeapImg1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fb2oRsc%2FbtqZZUAq5OE%2FAAAAAAAAAAAAAAAAAAAAACCMkxcdCyO5YnEVdeMMEtSYZOvSxZNki3w_rfjlisRi%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DttSlZ4m6Ulnzf6QVOo7%252BG9%252FSIns%253D)
![MaxHeapImg2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FYKTQv%2FbtqZYl6oJFd%2FAAAAAAAAAAAAAAAAAAAAAKfM-3GZRYm0wte6HXHVRlFnibBIa6B97i2zCiW4p-9f%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3Dm3XcFVYzfCOAR%252BN7jDZwWp5krQI%253D)

우선 순위 큐
우선 순위가 높은 데이터부터 먼저 처리하는 큐이다.
큐는 큐인데 우선순위가 높은 순위에 따라 pop하는 큐
(우선 순위 큐는 데이터의 값이 작을수록 우선 순위가 높다)

우선 순위 동작 원리
1. 빈 우선 순위 큐를 선언(형태는 큐와 동일)
2. 큐에 3 삽입(현재 빈 큐이기 때문에 우선 순위를 생각할 필요(X))
3. 이어서 1 삽입(먼저 들어와 있는 데이터와 비교하여 작으면 교환)
4. pop()을 이용하여 제거

힙으로 우선 순위 큐를 구현 => 
heappush()함수를 사용한다.
이 함수는 heapq 모듈을 가지고 있다.

```python
import heapq

# 빈 힙을 생성
heap = []

# 값을 우선 순위 큐에 삽입(heappush())
heapq.heappush(heap, 10)
heapq.heappush(heap, 5)
heapq.heappush(heap, 20)
heapq.heappush(heap, 1)

# 우선 순위 큐의 상태 출력
print(heap) # [1, 5, 10, 20]

# 우선 순위 큐에서 가장 작은 요소 확인 후 제거(heappop())
print(heapq.heappop(heap)) # 1출력
print(heap) # [5, 10, 20]
```

