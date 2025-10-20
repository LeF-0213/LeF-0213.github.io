---
layout: post
related_posts:
    - /frontend/python
title:  "자료 구조(비선형 자료구조)"
date:   2025-08-21
categories:
  - frontend
  - python
description: >
  예시를 통해 
---
* toc
{:toc .large-only}

# 비선형 자료구조(Nonelinear-DataStructure)
**데이터의 저장방식**: 데이터를 계층적 또는 네트워크 형태로 저장 및 탐색

# 트리(Tree)
![Tree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbbNRYk%2FbtqV36qaJux%2FAAAAAAAAAAAAAAAAAAAAAC_HRAtZ-SvslOfJ3Hv65XJYG2RMy2PWuROMQIjjcVLo%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DqQQSZD3fVwc8O5cgjl90LNsLgf8%253D)
* 계층적 구조로 데이터를 저장하는 자료구조
* **노드(Node)**와 **간선(Edge)**로 구성
* **특징**: 사이클이 없는 연결 그래프

## 트리의 기본 용어
* **루트 노드(root node)**: 루트 노드는 하나의 트리에서 하나밖에 존재하지 않고, 부모노드가 없다.(ex. 녹색 노드)
* **부모 노드(parent node)**: 자기 자신(노드)와 연결 된 노드 중 자신보다 높은 노드를 의미 (ex. F의 부모노드 : B)
* **자식 노드(child node)**: 자기 자신(노드)와 연결 된 노드 중 자신보다 낮은 노드를 의미 (ex. C의 자식노드: G, F)
* **단말 노드(leaf node)**: 리프 노드라고도 불리며 자식 노드가 없는 노드를 의미(ex. 주황색 노드)
* **내부 노드(internal node)**: 단말 노드가 아닌 노드
* **형제 노드(sibling node)**: 부모가 같은 노드를 말한다.(ex. D, E, F는 모두 부모노드가 B이므로 형제 노드이다.)
* **깊이(depth)**: 특정 노드에 도달하기 위해 거쳐가야 하는 **간선의 개수**를 의미(ex. F의 깊이: A -> B -> F 이므로 깊이는 2가 된다.)
* **레벨(level)**: 특정 깊이에 있는 노드들의 집합을 말하며, 루트를 0으로 하는 깊이이다.
* **차수(degree)**: 특정 노드가 하위(자식) 노드와 연결된 개수(ex. B의 차수 = 3)

# 이진 트리(Binary Tree)
* 각 노드가 **최대 2개의 자식**을 가지는 트리
* 왼쪽 자식(left)과 오른쪽 자식(right)로 구성
### 노드 구조
```python
class TreeNode:
	def __init__(self, item, left=None, right=None):
		self.item = item
		self.left = left
		self.right = right
```
### 이진 트리의 종류
![KindsOfBinaryTree](https://blogfiles.pstatic.net/MjAyMzAzMDhfMjkg/MDAxNjc4MjY1MTM1NTk5.4VVtF8d3_8Gy_2EcqQQqG2i5oaRUW8lgosk-OxkuxBgg.VUGWDRxLVtXkZC0Slej-5wJodQV4P2nQUB9wko7UnNkg.PNG.taeyang95/image.png?type=w1)
* **포화 이진 트리(Perfect Binary Tree)**: 모든 레벨이 완전히 채워진 트리
* **완전 이진 트리(Complete Binary Tree)**: 마지막 레벨을 제외하고 모두 채워지고, 왼쪽부터 채워진 트리
* **편향 트리(Skewed Tree)**: 한쪽으로만 치우친 트리
### 이진 트리의 배열 표현
* 이진 트리를 배열로 표현할 수 있으며, 인덱스 계산으로 부모-자식 관계를 파악할 수 있다.
* **장점**: 포인터 없이 인덱스 계산만으로 트리 구조 표현 가능
* **단점**: 편향 트리의 경우 메모리 낭비가 심함
1. 인덱스 1부터 시작 (인덱스 0 미사용)
```python
# 배열: [None, 3, 4, 7, 2, 8, 9, 1]
# 인덱스 0은 사용하지 않음

# 부모 노드의 인덱스가 i일 때
left_child = i * 2			# 왼쪽 자식
right_child = i * 2 + 1	# 오른쪽 자식
parent = i // 2					# 부모 노드
```
2. 인덱스 0부터 시작(루트 노드가 0)
```python
# 배열: [3, 4, 7, 2, 8, 9, 1]
# 인덱스 0부터 사용

# 부모 노드의 인덱스가 i일 때
left_child = i * 2 + 1    # 왼쪽 자식
right_child = i * 2 + 2   # 오른쪽 자식
parent = (i - 1) // 2     # 부모 노드
```
### 이진트리의 배열 사용 예시(전위 순회)
* 이진 트리로 되어있는 것을 배열로 표현하려고 하면 인덱스는 0이 아니라 1부터 시작(루트 노드)
* 루트를 기준으로 왼쪽 노드부터 시작
```python
def preorder(nodes, idx):
  if idx < len(nodes):
    ret = str(nodes[i]) + ""
    ret += preorder(nodes, idx * 2 + 1)
    ret += preorder(nodes, idx * 2 + 2)
    return ret
  else:
    return ""
```

# 이진 탐색 트리(Binary Search Tree, BST)
* **특징**: 왼쪽 자식 < 부모 < 오른쪽 자식
* **탐색 시간복잡도**: 평균 O(log n), 최악 O(n)
### BST의 ADT
* 속성:
  * `TreeNode root`: 트리의 루트 노드
* 주요 연산:
  * `insert(item)`:노드 삽입
  * `search(item)`:노드 탐색
  * `delete(item)`:노드 삭제
### 구현 예시
```python
class TreeNode:
	def __init__(self, item):
		self.item = item
		self.left = None
		self.right = None

class BST:
	def __init__(self):
		self.root = None

	def insert(self, item):
		if not self.root:
			self.root = TreeNode(item)
		else:
			self._insert_recursive(self.root, item)
	
	def _insert_recursive(self, node, item):
		if item < node.item:
			if node.left is None:
				node.left = TreeNode(item)
			else:
				self._insert_recursive(node.left, item)
		else:
			if node.right is None:
				node.right = TreeNode(item)
			else:
				self._insert_recursive(node.right, item)

	def search(self, item):
		return self._search_recursive(self.root, item)

	def _search_recursive(self, node, item):
		if node is None:
			return False
		if node.item == item:
			return True
		elif item < node.item:
			return self._search_recursive(node.left, item)
		else:
			return self._search_recursive(node.right, item)
```
## 트리 순회(Tree Traversal)
트리의 모든 노드를 방문하는 방법
### 전위 순회(Preorder): 부모노드 -> 왼쪽 자식노드 -> 오른쪽 자식노드
```python
def preorder(node):
	if node:
		print(node.item, end='')
		preorder(node.left)
		preorder(node.right)
```
### 중위 순회(Inorder): 왼쪽 자식노드 -> 부모노드 -> 오른쪽 자식노드
```python
def inorder(node):
	if node:
		inorder(node.left)
		print(node.item, end='')
		inorder(node.right)
```
### 후위 순회(Postorder): 왼쪽 자식노드 -> 오른쪽 자식노드 -> 부모노드
```python
def postorder(node):
	if node:
		postorder(node.left)
		postorder(node.right)
		print(node.item, end='')
```
### 레벨 순회(Level Order): 레벨별로 왼쪽에서 오른쪽
```python
from collections import deque

def levelorder(root):
	if not root:
		return

	queue = deque([root])
	while queue:
		node = queue.popleft()
		print(node.item, end='')

		if node.left:
			queue.append(node.left)
		if node.right:
			queue.append(node.right)
```
### 사용 예시 - 이전 트리의 깊이 구하기
```python
def maxDepth(root):
	if not root:
		return 0

	left_depth = maxDepth(root.left)
	right_depth = maxDepth(root.right)

	return max(left_depth, right_depth) + 1
```

# 힙(Heap)
* **완전 이진 트리** 기반의 자료구조
* **특징**: 부모 노드가 자식 노드보다 항상 크거나(최대 힙) 작음(최소 힙)
### 힙의 종류
![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FlR7aO%2FbtqZSuVD2vb%2FAAAAAAAAAAAAAAAAAAAAAP4RP2moe9XAe3MSmP4fO98kxAr9SJZxiLWgCrSM97cU%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DuBAXwZEON8THrpfzJZDoaCH%252F%252F%252BA%253D)
* 최대 힙: 부모 노드의 값(key 값) >= 자식 노드의 값(key 값)
* 최소 힙: 부모 노드의 값(key 값) <= 자식 노드의 값(key 값)
### 힙의 ADT
* 속성:
  * `list heap`: 힙을 저장하는 리스트
* 주요 연산:
  * `insert(item)`: 원소 삽입
  * `delete()`: 루트 노드 삭제(최댓값 또는 최솟값)
  * `heapify()`: 힙 속성 유지
## 힙의 동작 원리
힙은 **완전 이진 트리**이므로 배열로 표현하며, 부모-자식 관계는 인덱스로 계산한다.
### 직접 구현 시 인덱스 1부터 시작(구현의 용이함을 위해서)
* 부모 인덱스: `i // 2`
* 왼쪽 자식: `i * 2`
* 오른쪽 자식: `i * 2 + 1`
### heapq 모듈은 인덱스 0부터 시작
* 부모 인덱스: `(i - 1) // 2`
* 왼쪽 자식: `i * 2 + 1`
* 오른쪽 자식: `i * 2 + 2`
## 최소 힙(min heap)
![MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FWDMjZ%2FbtqV8GEMhcb%2FAAAAAAAAAAAAAAAAAAAAAF8CXqJXA3SMYhN20A22zI607t2-bLJRXWq9VGPHaFLK%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3Dr%252FAK72NiGxQBd5%252FtfLRJjzSURSU%253D)
### 최소 힙 삽입(Heapify Up)
1. 새 원소를 배열의 **맨 끝**에 추가
2. 부모 노드와 비교하여 **부모보다 작으면 교환**
3. 루트에 도달하거나 힙 속성을 만족할 때까지 반복
### 최소 힙 삭제(Heapify Down)
1. **루트 노드(최솟값)**를 삭제하고 반환
2. 배열의 **마지막 원소**를 루트로 이동
3. 자식 노드들과 비교하여 **더 작은 자식과 교환**
4. 리프에 도달하거나 힙 속성을 만족할 때까지 반복
### 직접 구현 예시(최소 힙) - 인덱스 1부터 시작
* **초기 상태**: [None, 3, 10, 35, 23, 19, 47, 60, 35, 80, 44]
```python
class MinHeap:
	def __init__(self):
		self.heap = [None] # 인덱스 0은 사용하지 않음
# push 메소드 구현
	def push(self, item):
		self.heap.append(item) # 1. 맨 끝에 추가
		self._heapify_up(len(self.heap) - 1) # 2. Heapify Up

	def _heapify_up(self, idx):
		if idx <= 1:	# 루트에 도달하면 종료
			return

		parent = idx // 2

		# 부모보다 작으면 교환
		if self.heap[idx] < self.heap[parent]:
			self.heap[idx], self.heap[parent] = self.heap[parent], self.heap[idx]
			self._heapiy_up(parent)
# pop메서드 구현	
	def pop(self):
		# 경우 1: 힙이 비어있음 (None만 있음)
		if len(self.heap) <= 1:
			return None

		# 경우 2: 원소가 1개만 있음 [None, 5]
		if len(self.heap) == 2:
			return self.heap.pop() # 5를 반환하고 제거

		# 경우 3: 원소가 2개 이상 [None, 3, 10, 35, ...] 위의 그림 참조
		# 1. 루트(최솟값) 저장
		root = self.heap[1]		# 3을 저장
		# 2. 마지막 원소를 루트 위치로 이동(마지막 원소 44를 꺼내서 루트에 넣음)
		self.heap[1] = self.heap.pop() # [None, 44, 10, 35, 23, 19, 47, 60, 35, 80]
		# 3. Heapify Down
		self._heapify_down(1)

		return root

	def _heapify_down(self, idx):
		smallest = idx # 1 (값: 44)
		left = idx * 2 # 1 * 2 = 2 (값 : 10)
		right = idx * 2 + 1 # 1 * 2 = 1 = 3 (값 : 35)

		# 왼쪽 자식과 비교
		# left < len(self.heap): 왼쪽 자식이 존재하는 지 확인
		#	self.heap[left] < self.heap[smallest]: 왼쪽 자식이 더 작은지 확인(10 < 44)
		if left < len(self.heap) and self.heap[left] < self.heap[smallest]:
			smallest = left # 2

		# 오른쪽 자식과 비교
		if right < len(self.heap) and self.heap[right] < self.heap[smallest]:
			smallest = right

		# 교환이 필요하면 재귀
		if smallest != idx:
			self.heap[idx], self.heap[samllest] = self.heap[smallest], self.heap[idx]
			self._heapify_down(smallest) 

# 사용 예시
min_heap = MinHeap()
for num in [23, 10, 35, 19, 47, 60, 35, 80, 44]:
	min_heap.push(num)
print(min_heap.heap[1:])	# [3, 10, 35, 23, 47, 60, 35, 80, 44]
```

### 직접 구현(최대 힙)
```python
class MaxHeap:
	def __init__(self):
		self.heap = [None] # 인덱스 0은 사용하지 않음

	def push(self, item):
		self.heap.append(item)
		self._heapify_up(len(self.heap) - 1)

	def _heapify_up(self, idx):
		if idx <= 1:
			return

		parent = idx // 2

		# 부모보다 크면 교환
		if self.heap[idx] > self.heap[parent]:
			self.heap[idx], self.heap[parent] = self.heap[parent], self.heap[idx]
			self._heapify_up(parent)

	def pop(self):
		if len(self.heap) <= 1:
			return None

		if len(self.heap) == 2:
			return self.heap.pop()

		root = self.heap[1]
		self.heap[1] = self.heap.pop()
		self._heapify_down(1)

		return root

	def _heapify_down(self, idx):
		largest = idx
		left = idx * 2
		right = idx * 2 + 1

		# 왼쪽 자식과 비교
		if left < len(self.heap) and self.heap[left] > self.heap[largest]:
			largest = left

		# 오른쪽 자식과 비교
		if right < len(self.heap) and self.heap[right] > self.heap[largest]:
			largest = right

		if largest != idx:
			self.heap[idx], self.heap[largest] = self.heap[largest], self.heap[idx]
			self._heapify_down(largest)

# 사용 예시
max_heap = MaxHeap()
for num in [23, 10, 35, 19, 47, 60, 35, 80, 44]:
	max_heap.push(num)
print(max_heap.heap[1:])	# [80, 60, 47, 44, 35, 35, 19, 23, 10]
```

### heapq 모듈 사용
```python
import heapq

# 최소 힙
min_heap = []	# 빈 리스트 생성
heapq.heappush(min_heap, 5) # [5]
heapq.heappush(min_heap, 3) #	[3, 5]
heapq.heappush(min_heap, 7) # [3, 5, 7]
heapq.heappush(min_heap, 1) # [1, 3, 7, 5]
min_value = heapq.heappop(min_heap) # 1

# 최대 힙: 음수로 변환
max_heap = []
heapq.heappush(max_heap, -5) # [-5]
heapq.heappush(max_heap, -3) # [-5, -3]
heapq.heappush(max_heap, -7) # [-7, -3, -5]
heapq.heappush(max_heap, -1) # [-7, -3, -5, -1]
max_value = -heapq.heappop(max_heap) # 7
```
### 사용 예시 - 최소 힙으로 K번째 작은 수 찾기
```python
import heapq

def kthSmallest(nums, k):
	heapq.heapify(nums)

	for _ in range(k - 1):
		heapq.heappop(nums)

	return heapq.heappop(nums)
```

# 그래프(Graph)
* **노드(정점, Vertex)**와 **간선(Edge)**로 구성된 네트워크 구조
* **특징**: 사이클이 있을 수 있음(순환 가능)
## 그래프의 종류
* **무방향 그래프(Undirected Graph)**: 간선에 방향이 없음
* **방향 그래프(Directed Graph)**: 간선에 방향이 있음
* **가중치 그래프(Weighted Graph)**: 간선에 가중치(비용)가 있음
## 그래프 표현 방법
### 인접 행렬(Adjacency Matrix, 2차원 배열)
* 무방향 그래프
```python
graph[0][1] = graph[1][0] = 1 # 0 -> 1, 1 -> 0 모두 가능
```
* 방향 그래프
```python
graph[0][1] = 1 # 0 -> 1만 가능
```
#### 구현 예시(가중치 X)
```python
# 정점 4개인 그래프
n = 4
graph = [[0] * n for _ in range(n)]
"""
graph = [
	[0, 0, 0, 0],
	[0, 0, 0, 0],
	[0, 0, 0, 0],
	[0, 0, 0, 0]
]
"""

# 간선 추가 (0-1, 0-2, 1-3)
graph[0][1] = graph[1][0] = 1
graph[0][2] = graph[2][0] = 1
graph[1][3] = graph[3][1] = 1
"""
graph = [
	[0, 1, 1, 0],
	[1, 0, 0, 0],
	[1, 0, 0, 0],
	[0, 0, 0, 0]
]
"""
```
#### 구현 예시(가중치 O)
```python
n = 4
INF = float('inf') # 연결되지 않은 경우를 표현
graph = [[0] * n for _ in range(n)]

# 자기 자신으로 가능 비용은 0
for i in range(n):
	graph[i][i] = 0

# 간선 추가 (점점1, 점정2, 가중치)
graph[0][1] = graph[1][0] = 5
graph[0][2] = graph[2][0] = 3
graph[1][3] = graph[3][1] = 7

"""
graph = [
	[0, 5, 3, INF],
	[5, 0, INF, 7],
	[3, INF, 0, INF],
	[INF, 7, INF, 0]
]
"""
```
### 인접 리스트(Adjacency List, Linked List)
* 무방향 그래프
```python
graph[0].append(1)
graph[1].append(0)
```
* 방향 그래프
```python
graph[0].append(1)
```
#### 구현 예시(가중치 X)
```python
n = 4
graph = [[] for _ in range(n)]
"""
graph = [
	[],
	[],
	[],
	[]
]
"""

# 간선 추가(0-1, 0-2, 1-3)
graph[0].append(1)
graph[1].append(0)
graph[0].append(2)
graph[2].append(0)
graph[1].append(3)
graph[3].append(1)
"""
graph = [
	[1, 2], # 0번 정점: 1, 2와 연결
	[0, 3],
	[0],
	[1]
]
"""
```
#### 구현 예시(가중치 O)
```python
n = 4
graph = [[] for _ in range(n)]

# 간선 추가: (도착 정점, 가중치) 튜플로 저장
graph[0].append((1, 5))
graph[1].append((0, 5))
graph[0].append((2, 3))

```
## 그래프 탐색
### 깊이 우선 탐색(DFS, Depth First Search)



그래프 탐색
깊이 우선 탐색
더 이상 탐색 할 노드가 없을 때까지 탐색(일단 한 방향으로)
너비 우선 탐색
현재 노드를 기준으로 가장 가까운 노드부터 모두 방문하고 다음 노드로 이동

깊이 우선 탐색(DFS) => 스택 방식에 의해 구현
너비 우선 탐색(BFS) => 큐 방식에 의해 구현
완전 탐색(모든 노드를 방문)

## DFS(깊이 우선 탐색)
탐색 시작 노드를 스택에 삽입하고 방문 처리 한다.
스택의 최상단 노드 중 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 삽입하고 방문 처리
2번의 과정을 더  방문할 노드가 없을 때까지 반복

1. 스택을 생성한다.(빈 스택)
stack = []
노드 중에서 가장 작은 데이터를 잡는다.
2. 방문 노드를 기준으로 방문하지 않는 인접 노드를 탐색
(그 중 노드 값이 작은 노드를 스택에 삽입 후 방문 처리)
3. 마지막으로 방문한 노드를 기준
인접 노드 중 방문하지 않은 노드를 스택에 삽입하고 방문 처리

위의 방식으로 탐색 한 순서는
1 -> 2 -> 7 -> 6 -> 8 -> 3 -> 4 -> 5

```python
def dfs(graph, v, visited):
  visited[v] = True     # 현재 노드 v를 방문 처리
  print(v, end = '')    # 방문한 노드 출력

  for i in graph[v]:    # 현재 노드 v와 연결된 노드들을 확인
    if not visited[i]:  # 아직 방문하지 않았다면
      dfs(graph, i, visited)  # 재귀적으로 방문
```
매개변수 의미
* **graph**: 인접 리스트
* **V**: 현재 방문할 노드 번호
* **visited**: 방문 여부를 저장하는 리스트

## BFS(너비 우선 탐색)
가까운 노드부터 탐색하는 알고리즘

```python
from collections import deque

def bfs(graph, start, visited):
  queue = deque([start])
  visited[start] = True

  while queue:
    v = queue.popleft()
    print(v, end = '')
    for i in graph[v]:
      if not visited[i]:
        queue.append(i)
        visited[i] = True
```

정수 배열 num이 주어진다. 이 num에서 서로 다른 인덱스에 있는 2개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 반환하는 함수를 작성하시오(함수 이름은 solution()이다.) (중복 값은 허용하지 않는다.)

num = [2, 1, 3, 4, 1]
num = [5, 0, 2, 7]

```python
def sol(num):
  result = []

  for i in range(len(num)):
    for j in range(i + 1, len(num)):
      result.append(num[i] + num[j])
  
  result = sorted(set(result))
  return result
```

# 비선형 자료구조
**데이터의 저장방식**: 데이터를 저장하는 순서가 정해져있지 않다. 
## 트리 
- 그래프

### 그외
- sort





https://school.programmers.co.kr/learn/courses/30/lessons/42840 
https://school.programmers.co.kr/learn/courses/30/lessons/12949


```python
num1 = int(input())  
num2 = int(input())

result = num1 + num2
print(result)
```

`input()` 함수는 입력되는 모든 값을 문자열로 인식한다.


## Tree(이진 트리)
루트 노드
노드
간선
단말 노드(자식이 없는 노드)

List의 객체 구조
파이썬에서는 리스트 자료구조가 기본적으로 제공(내부에서 어떻게 돌아가는지 알 필요(X))
ADT 리스트
inser(), append(), pop(), remove(), index(), clear(), count(), extend(), copy(), reverse(), sort()

num = [1, 3, 5, 7]

## 원소 삽입
### insert()
리스트의 인덱스 번호에 데이터를 삽입
```python
num.insert(1, 8)
num
# [1, 8, 3, 5, 7]
```

### append()
리스트의 마지막에 데이터를 삽입
```python
num.append(8)
num
# [1, 3, 5, 7, 8]
```

## 원소 삭제
### pop()
리스트와 스택에 따라 사용이 달라짐
```python
num.pop(3)
num
# [1, 3, 7]
```

```python
num.pop()
num
# [1, 3, 5]
```

```python
num.pop(-1)
num
# [1, 3, 5]
```

### remove()
삭제할 원소를 직접적으로 선택
```python
num.remove(5)
num
# [1, 3, 7]
```

## 리스트 비우기
### clear()
```python
num.clear()
num
# []
```

리스트, 큐, 스택 ==> 반드시 빈 리스트, 큐, 스택을 생성

연결 리스트 
배열의 공간 낭비를 피할 수 있는 자료구조    
원소를 추가할 때마다 공간을 할당받아 추가하는 동적 할당 방식    
원소를 저장하는 최소 단위 = 노드

data, item | 포인터(c언어), reference(java)

마지막 노드라고 가정하면 더 이상 노드가 없다라는 의미로 None를 할당

head => 연결 리스트에서 첫번째 노드에 대한 레퍼런스
numItems => 연결 리스트에 들어있는 원소의 총 수

newNode.next = pre.next
pre.next = newNode

```python
if i == 0:
  newNode.item = x
  newNode.next = head
  head = newNode
  numItem == 1
else:
  newNode.tiem = x
  newNode.next = pre.next
  pre.next = newNode
  numItem += 1
```

```python
class ListNode:
  def __init__(self, newItem, nextNode):
    self.item = newItem
    self.next = nextNode
```

```python
class LinkedListBasic:
	def __init__(self):
		self.__head = ListNode('dummy', None)
		self.__numItems = 0

	def insert(self, i:int, newItem):
		if i >= 0 and i <= self.__numItems: # 노드의 범위
			prev = self.__getNode(i - 1)  # 앞선 노드
			newNode = ListNode(newItem, prev.next) # 새로운 노드의 item값과, 연결된 주소
			prev.next = newNode # prev의 다음 노드로 위치
			self.__numItems += 1 # 추가된 노드 수 더하기
		else:
			print("index", i, ": out of bound in insert()") 
 
	def append(self, newItem):
		prev = self.__getNode(self.__numItems - 1)  # 끝에서 앞인 노드
		newNode = ListNode(newItem, prev.next)
		prev.next = newNode
		self.__numItems += 1


	def pop(self, i:int):  
		if (i >= 0 and i <= self.__numItems-1): 
			prev = self.__getNode(i - 1)
			curr = prev.next # 주소값을 prev 다음으로 위치
			prev.next = curr.next # 원래 prev 다음 값을 현재 주소값의 다음으로 위치
			retItem = curr.item
			self.__numItems -= 1
			return retItem
		else:
			return None
	
	def remove(self, x):
		(prev, curr) = self.__findNode(x)
		if curr != None:
			prev.next = curr.next
			self.__numItems -= 1
			return x
		else:
			return None

	def get(self, i:int):
		if self.isEmpty():
			return None
		if (i >= 0 and i <= self.__numItems - 1):
			return self.__getNode(i).item
		else:
			return None
 
	def index(self, x) -> int:
		curr = self.__head.next	 
		for index in range(self.__numItems):
			if curr.item == x:
				return index
			else:
				curr = curr.next
		return -2 # 

	def isEmpty(self) -> bool:
		return self.__numItems == 0

	def size(self) -> int:
		return self.__numItems

	def clear(self):
		self.__head = ListNode("dummy", None)
		self.__numItems = 0

	def count(self, x) -> int:
		cnt = 0
		curr = self.__head.next  
		while curr != None:
			if curr.item == x:
					cnt += 1
			curr = curr.next
		return cnt

	def extend(self, a): 
		for index in range(a.size()):
			self.append(a.get(index))
 
	def copy(self):
		a = LinkedListBasic()
		for index in range(self.__numItems):
			a.append(self.get(index))
		return a

	def reverse(self):
		a = LinkedListBasic()
		for index in range(self.__numItems):
			a.insert(0, self.get(index))
		self.clear()
		for index in range(a.size()):
			self.append(a.get(index))

	def sort(self) -> None:
		a = []
		for index in range(self.__numItems):
			a.append(self.get(index))
		a.sort()
		self.clear()
		for index in range(len(a)):
			self.append(a[index])
 
	def __findNode(self, x) -> (ListNode, ListNode):
		prev = self.__head  # 더미 헤드
		curr = prev.next    # 0번 노드
		while curr != None:
			if curr.item == x:
				return (prev, curr)
			else:
				prev = curr; curr = curr.next
		return (None, None)

	def __getNode(self, i:int) -> ListNode:
		curr = self.__head # 더미 헤드, index: -1
		for index in range(i+1):
			curr = curr.next
		return curr

	def printList(self):
		curr = self.__head.next 
		while curr != None:
			print(curr.item, end = ' ')
			curr = curr.next
		print()
```

완전 탐색
DFS(stack) 깊이 우선 탐색
BFS(queue) 너비 우선 탐색
재귀 함수
↓
탐색 알고리즘
1. 인접 행렬
2차원 배열로 연결관계를 표현 
2. 인접 리스트
리스트로 연결 관계를 표현
=> 트리 그래프

노드(vertex)
간선
가중치

### 인접 행렬 
* 2차원 배열에 각 노드가 연결된 형태를 기록
* 파이썬에서는 2차원 리스트
* 연결 되지 않는 노드끼리는 무한의 비용(INF = 999999999)

   0  1  2
0  0  5  2
1  5  0 INF
2  2 INF 0
파이썬의 2차원 리스트 인접 행렬
```python
graph = [
    [0, 5, 2],
    [5, 0, INF],
    [2, INF, 0]
  ]
```

## DFS
stack 자료 구조를 이용하여 구체적인 동작을 한다.
1. 탐색 시작노드를 스택에 삽입하고 방문 처리한다.(노드에 다시 삽입되지 않게 하기 위해)
2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면, 그 인접 노드를 스택에 넣고 방문 처리 한다. (stack 방식이 적용)
3. 2번 과정을 더 이상 수행할 수 없을 때까지 반복한다.(단말노드를 만날 때까지)

코딩테스트에서는 번호가 낮은 순서부터 처리한다.

먼저 공백 스택을 생성

1. 시작 노드 1을 스택에 삽입하고, 방문 처리를 한다.
2. 스택의 최상단 노드 1을 기준으로 인접한 노드를 찾는다. (2, 3, 8 노드) 이 중에서 가장 작은 노드를 스택에 삽입하고 방문 처리
3. 스택의 최상단 노드 2을 기준으로 방문하지 않은 인접한 노드를 찾는다. (7, 8 노드) 그 중 가장 작은 노드의 값을 스택에 삽입 후 방문 처리 
4. 스택의 최상단 노드 7을 기준으로 방문하지 않은 인접한 노드를 찾는다. (6, 8 노드) 6을 스택에 삽입 후 방문 처리
5. 6이 단말 노드 이므로, 6을 빼고 다시 7을 기준으로 방문하지 않는 인접한 노드를 찾는다. 8을 스택에 삽입 후 방문 처리
6. 더 이상 인접 노드가 없기에 8, 7, 2다시 빼내고, 1에 인접한 노드 3을 스택에 삽입 후 방문 처리한다.
7. 스택의 최상단 노드 3을 기준으로 방문하지 않은 인접한 노드를 찾는다. (4, 5 노드) 4을 스택에 삽입 후 방문 처리
8. 4와 인접한 노드 5을 스택에 삽입 후 방문 처리
노드의 탐색 순서는 1 -> 2 -> 7 -> 6 -> 8 -> 3 -> 4 -> 5

```python
def dfs(graph, n, visited):
  visited(n) = True
  print(n, end = ' ')

# 현재 노드와 다른 노드를 재귀를 통해 방문
  for i in graph[n]:
    if not visited[i]:
      dfs(graph, i, visited)

# 각 노드가 연결된 정보를 리스트 자료형으로 표현
graph = [

]

visited = [False] * 9

dfs(graph, 1, visited)
```

## BFS 너비 우선 탐색
가까운 노드부터 탐색
DFS(깊이 우선 탐색)는 최대한 멀리 있는 노드를 우선으로 탐색하는 방식으로 동작한다.
BFS는 반대 개념이다.
Queue 방식을 사용하여 구현한다.

구현하는 방법
1. 빈 큐를 생성
2. 탐색 시작 노드를 큐에 삽인한 후 방문 처리한다.
3. 큐에서 노드를 꺼내 해당 인접 노드 중에서 방문하지 않은 모든 노드를 모두 큐에 삽입한다. 그리고 나서 방문 처리(Queue 방식)
4. 3번 과정을 더 이상 수행할 수 없을 때까지 반복

먼저 공백 `Queue`를 생성
1 -> 2 -> 3 -> 8 -> 7 -> 4 -> 5 -> 6

```python
def bfs(graph, n, visited):
  visited(n) = True
  print(n, end = " ")
  nexts = []

  for i in graph[n]:
    if not visited[i]:
      visited[i] = True
      print(n, end = " ")
      nexts.append(i)
  bfs(graph, nexts, visited)

graph = []

visited = [Fasle] * 9
```

# 집합(그래프에서 아주 많이 활용하는 것)
순서 및 중복이 없는 원소들을 갖는 자료구조
예를 들어 A = {1, 6, 6, 6, 4, 3} ==> 집합으로 생각할 때
A = {1, 6, 4, 3} => 중복 제거
A = {6, 4, 1, 3} => 순서 X

### 집합은 특성에 따라 부르는 형태가 다양하게 존재
- 원소의 개수가 유한 => 유한 집합(반대 개념으로는 무한 집합)
- 아무런 원소를 가지고 있지 않으면 => 공집합
- 코테에서 출제되는 집합 => 상호베타적 집합(교집합이 없는 집합 관계)

교집합이란?
위의 상호배타적 집합을 사용하여 코딩하는
- 최소 신장 트리(노드의 개수가 간선의 개수보다 하나 작고, 순환이 안 됨)
- 네트워크
- 게임 개발

### 집합의 연산
집합은 트리로 표현하며 대표적인 연산 합치기와 탐색이 존재한다.

### 트리를 배열로 표현하기(빈 값이 생김) -> 인접 리스트 표현

### 표현
집합은 배열을 활용한 트리로 구현된다
**각각의 집합에는 대표 원소가 있어야 한다**

집합을 배열로 표현한다는 의미는 하나의 배열로 상호 배타적 관계를 가지는 집합을 모두 표현하게 된다
- **배열의 인덱스는 현재 노드의 값을 의미하고, 배열 값은 부모 노드를 의미한다.**

예를 들어보면 a[3] = 10
-> 노드 3의 부모 노드는 10이다.
루트 노드는 집합의 대표이므로 부모가 없다. 부모 노드가 자기 자신이다.
(루트 노드는 값 자체가 배열의 인덱스와 동일하다)

(배열의 인덱스는 0부터 시작하는데 집합을 배열로 표현할 때에는 0을 사용하지 않는다)

부모 노드의 값을 배열의 값으로 저장

현재 노드의 값을 배열의 인덱스로 활용

# 유니온-파인드 알고리즘
### 파인드 연산 
특정 노드와 루트 노드가 무엇인지 탐색하는 연산
(특징으로는 특정 노드가 같은 집합에 있는지 확인할 때 사용한다)



