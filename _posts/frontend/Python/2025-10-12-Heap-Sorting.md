---
layout: post
related_posts:
    - /frontend/python
title:  "힙 정렬"
date:   2025-08-27
categories:
  - frontend
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

# Heap이란?
힙(Heap)은 **우선순위 큐**를 구현하기 위한 자료구조이다. 힙을 뜻을 살펴보면, "쌓아 올린 더미"라는 의미를 가진다. 힙은 **완전 이진 트리** 형태로 구성된다.

## 완전 이진 트리(Complete Binary Tree)란?
![CompleteBinaryTree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbfi34R%2FbtqV2WnM4Jj%2FAAAAAAAAAAAAAAAAAAAAALd34KSANB_SeFQH5a5Vyzp66U-iBwLiBStH0ga-UBtQ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1764514799%26allow_ip%3D%26allow_referer%3D%26signature%3D%252F263QoK6Pewpc%252BB30vAv0FKbuh8%253D)
#### 이진 트리(Binary Tree)

> 트리 구조에서 특정한 형태로 제한을 하게 되는데, **모든 노드의 최대 차수를 2로 제한 하는 것이다.** 즉, 각 노드는 자식노드를 최대 2개까지 밖에 못 갖는 것인데, 이를 **이진 트리(Binary Tree)**라고 한다.    
    
#### 완전 이진 트리(Complete Binary Tree)

> **완전 이진 트리**는 **마지막 레벨**을 제외한 모든 노드가 체외져 있으면 모든 노드가 왼쪽부터 채워져 있어야한다.     
> 즉, 완전 이진 트리는 이진 트리에서 두가지 조건을 더 만족해야 한다.      

> **규칙 1**: 노드를 왼쪽에서 오른쪽으로 하나씩 빠짐없이 채워나간다.(레벨 순서로 노드를 삽입한다.)      
![heap1](https://wikidocs.net/images/page/194445/ds-059.png)
* 위 그림 모두 이진트리인데, a는 힙이지만 b는 힙이 아니다.          
* 왼쪽부터 채워야 한다는 규칙을 벗어났기 때문이다.

> **규칙 2**: 최소 힙은 부모 노드가 자식 노드의 값보다 작거나 같아야 한다. 파이썬의 heapq 모듈은 최소 힙(min heap)이다. (최대 힙은 부모 노드가 자식 노드의 값보다 크거나 같다.)

#### 포화 이진 트리(Perfect Binary Tree)

> 추가적으로 완전 이진 트리에서 마지막 레벨을 제외한 모든 노드는 모두 두개의 자식을 갖는다는 조건을 덧붙이면, **포화 이진 트리(Perfect Binary Tree)**가 된다.

> 즉, heap은 **최솟값** 또는 **최댓값**을 **빠르게** 찾아내기 위해 **완전이진트리** 형태로 만들어진 자료구조이다.

## Heap의 종류
![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FlR7aO%2FbtqZSuVD2vb%2FAAAAAAAAAAAAAAAAAAAAAP4RP2moe9XAe3MSmP4fO98kxAr9SJZxiLWgCrSM97cU%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DuBAXwZEON8THrpfzJZDoaCH%252F%252F%252BA%253D)

### 최소 힙(Min Heap)

> **특징**: 부모 노드의 값(key 값) <= 자식 노드의 값(key 값)      
> **루트 노드**: 가장 작은 값
> **용도**: 최솟값을 빠르게 찾을 때 사용

### 최대 힙(Max Heap)

> **특징**: 부모 노드의 값(key 값) >= 자식 노드의 값(key 값)       
> **루트 노드**: 가장 큰 값
> **용도**: 최댓값을 빠르게 찾을 때 사용    

## Heap을 배열로 구현하기 
![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbaNi4n%2FbtqZ2csFHgz%2FAAAAAAAAAAAAAAAAAAAAABL6CYDeUa-Y5KmmgEwMQDXYxEwHLHG31Tpr_RgFltZ4%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DTGktBDy5FnF7TIcYVkyOIoOanH0%253D)

### 특징

> 구현의 용이함을 위해 시작 인덱스(root)는 1부터 시작한다.      
> 각 노드와 대응되는 인덱스는 '불변한다'

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

