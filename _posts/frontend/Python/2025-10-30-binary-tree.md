---
layout: post
related_posts:
    - /frontend/python
title:  "[자료 구조]이진 트리(Binary Tree)"
date:   2025-10-29
categories:
  - frontend
  - python
description: >
  트리와 이진트리의 개념에 대해서 알아보고, 이진트리의 종류와 순회방법 및 실제 활용 사례를 들어 이해 및 코드 구현
---
* toc
{:toc .large-only}

# 트리(Tree)란?
![Tree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FSIzoC%2FbtrLuxTB3aY%2FAAAAAAAAAAAAAAAAAAAAAFSbui17w2MnpjfSi6kA7TM8Wb1Xtf9thoDfUjWiFvDa%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1764514799%26allow_ip%3D%26allow_referer%3D%26signature%3DvzDS94xRJVbUhW9u5Jrq15xr84Y%253D)

> **계층적 구조**를 표현하는 **비선형 자료구조**이다.     
> **노드(Node)들이 간선(Edge)으로 연결**되어 있으며,      
> **사이클이 없는 연결 그래프**이다.

## 트리의 기본 용어

| 용어 | 정의 | 예시 |
|------|------|------|
| 루트 노드 (root node) | 트리에서 하나만 존재하며 부모 노드가 없는 노드 | 녹색 노드 |
| 부모 노드 (parent node) | 자신과 연결된 노드 중 자신보다 높은 노드 | F의 부모 노드: B |
| 자식 노드 (child node) | 자신과 연결된 노드 중 자신보다 낮은 노드 | C의 자식 노드: G, F |
| 단말 노드 (leaf node) | 자식 노드가 없는 노드 | 주황색 노드 |
| 내부 노드 (internal node) | 단말 노드가 아닌 노드 | A, B, C, D, F, H |
| 형제 노드 (sibling node) | 부모가 같은 노드 | D, E, F는 모두 B의 자식이므로 형제 |
| 깊이 (depth) | 특정 노드에 도달하기 위해 거쳐야 하는 간선의 개수 | F의 깊이: A -> B -> F, 깊이 = 2 |
| 레벨 (level) | 특정 깊이에 있는 노드들의 집합 | 깊이 2에 있는 노드들의 집합 |
| 차수 (degree) | 특정 노드가 가진 자식 노드의 개수 | B의 차수 = 3 |
| 서브트리 (subtree) | 특정 노드와 그 자손들로 이루어진 부분 트리 | F, J, K |

# 이진 트리(Binary Tree)란?
![binaryTree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FWl43C%2FbtrLwAIRG4E%2FAAAAAAAAAAAAAAAAAAAAAEwtCAhiYbbS_0HwCePw93WzRqn_2FUcvWzvERWuUBuH%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DC0bS%252BaAje67%252B0Yx8t2VzBZWU79c%253D)

> 앞에서 본 트리 구조에서 특정한 형태로 제한을 하게 되는데, **모든 노드의 최대 차수를 2로 제한한 것**이다.    
> 즉, **각 노드가 최대 2개의 자식 노드까지 밖에 못 갖는다.**   
> **일반 이진 트리**는 레벨 순서로 왼쪽부터 채워나간다.  

## 이진 트리 vs 이진 탐색 트리
> 그런데 만약 위의 이진트리의 각 노드가 가지고 있는 값이 규칙없이 들어가게 된다면,
특정 값을 찾기 위해서는 최악의 경우 각각의 노드를 모두 순회(탐색)해야 된다.
이를 방지하고자 하나의 조건을 더 붙이게 되는데 이것을 우리는 **이진 탐색 트리**라고 부른다. 즉, 이진 트리에서 탐색에 더 특화된 트리를 말하는 것이다.

| 구분 | 이진 트리 (Binary Tree) | 이진 탐색 트리 (BST, Binary Search Tree) |
|:------:|------------------------|-----------------------------------------|
| 정의 | 각 노드가 최대 2개의 자식을 가짐 | 이진 트리 + 정렬 규칙 |
| 규칙 | 없음 (자유로운 배치) | 왼쪽 < 부모 < 오른쪽 |
| 목적 | 계층 구조 표현 | 빠른 검색 (O(log n)) |
| 삽입 | 레벨 순서로 | 값 비교해서 위치 결정 |
| 검색 | O(n) - 전체 탐색 필요 | O(log n) - 이진 탐색 |


# 이진 탐색 트리(Binary Search Tree)란?

> 이진 탐색 트리(BST)를 만들기 위해 추가 된 조건은 **부모 노드를 기준으로 왼쪽 자식 노드들은 부모 노드보다 값이 작으며, 오른쪽 자식 노드들은 부모노드보다 값이 크다.**는 것이다.      
> 즉, 찾으려는 노드의 값의 대소 관계를 비교해서 작다면 왼쪽 노드로 탐색하러 가면 되고, 반대로 크면 오른쪽 노드로, 같다면 그 값을 찾게 된 것이다.     

## 이진 트리의 종류
### 정 이진 트리(Full Binary Tree)
![FullBinaryTree](https://github.com/user-attachments/assets/9f743869-8407-4b70-879d-c3fbffd5414c)

> **모든 노드가 0개 또는 2개의 자식을 가지는 트리이다.**

### 완전 이진 트리(Complete Binary Tree)
![CompleteBinaryTree](https://github.com/user-attachments/assets/66f6d6f3-ec13-492b-b21b-b1cde601d0a7)

> **마지막 레벨을 제외한 모든 레벨이 완전히 채워져 있고, 마지막 레벨은 왼쪽부터 채워진 트리이다.**

### 포화 이진 트리(Perfect Binary Tree)
![perfectbinary](https://github.com/user-attachments/assets/2f18549f-8e4d-44fc-9c5e-acb03ba46aa8)

> **모든 레벨이 완전이 채워진 트리이다. (노드 개수 = 2^h - 1)**

### 편향 이진 트리(Skewed Binary Tree)
![skewedbinary](https://github.com/user-attachments/assets/214e5830-3960-4d75-8f41-95270fb86568)

> **모든 노드가 한쪽 방향으로만 자식을 가지는 트리이다.**

### ⭐️ 핵심 포인트
![TreeType](https://github.com/user-attachments/assets/433e3239-57cd-493a-8e7a-5f523b832d45)

> 정이진트리, 완전이진트리, 포화이진트리 등 **"형태"**를 분류하는 것이고, 일반 이진 트리와 이진 탐색 트리는 **"규칙"**을 분류하는 것이다. **두 분류는 독립적이므로 조합이 가능하다.**

![비교](https://github.com/user-attachments/assets/ee803ccf-1c31-4508-81d7-9a6418a821a2)
## 이진 트리의 순회(Traversal) 방법

> 트리의 모든 노드를 체계적으로 방문하는 방법으로, 크게 **전위 순회(Preorder Traversal)**, **중위 순회(Inorder Traversal)**, **후위 순회(Postorder Traversal)** 등이 있다.

### 전위 순회(Preorder Traversal)
![preorder](https://user-images.githubusercontent.com/81945553/175773688-a337d519-c525-4542-818b-46530885c066.png)

> 순서: 루트 -> 왼쪽 서브트리 -> 오른쪽 서브트리    
> 부모 노드를 선 방문 후 좌측 서브 트리와 우측 서브 트리를 방문하는 방식이다.     
> 리프 노드에 도착한 경우 백트래킹 관점으로 이전 분기점으로 이동한다.

```python
class Node:
    def __init__(self, item):
        self.item = item
        self.left = None
        self.right = None

def preorder(node):
  if node:
    print(node.item, end=' ')
    preorder(node.left)
    preorder(node.right)

# 노드 인스턴스 생성
node_A = Node("A")
node_B = Node("B")
node_C = Node("C")
node_D = Node("D")
node_E = Node("E")
node_F = Node("F")
node_G = Node("G")
node_H = Node("H")
node_I = Node("I")
node_J = Node("J")
node_K = Node("K")

# 생성한 노드 인스턴스들 연결
node_A.left = node_B
node_A.right = node_C

node_B.left = node_D
node_B.right = node_E

node_C.left = node_F
node_C.right = node_G

node_D.left = node_H

node_E.left = node_I
node_E.right = node_J

node_G.right = node_K

# 루트 노드 지정
root_node = node_A

preorder(root_node)

# 출력
A B D H E I J C F G K
```
### 중위 순회(Inorder Traversal)
![inorder](https://user-images.githubusercontent.com/81945553/175773778-0651c4a3-f1d9-4459-834d-2c8cbcfabf2d.png)

> 순서: 왼쪽 서브트리 -> 루트 -> 오른쪽 서브트리        
> 좌측 서브트리를 따라 왼쪽 리프 노드를 찍고 다시 부모 방문 후 오른쪽 노드 찍는 방식으로,     
> 좌측 서브트리가 끝나면 우측 서브트리도 동일한 방식으로 방문한다.

```python
def inorder(node):
  if node:
    inorder(node.left)
    print(node.item, end=' ')
    inorder(node.right)

# 출력
H D B I E J A F C G K
```
### 후위 순회(Postorder Traversal)
![postorder](https://user-images.githubusercontent.com/81945553/175773832-1874b598-c2be-48dd-af3f-9f37159d436f.png)

> 순서: 왼쪽 서브트리 -> 오른쪽 서브트리 -> 루트        
> 좌측 서브트리를 따라 왼쪽 자식노드 오른쪽 자식 노드를 방문하고 부모를 방문하는 식으로 좌측 서브트리가 끝나면,       
> 우측 서브트리도 동일한 방식으로 노드를 방문한 후, 부모를 방문한다.

```python
def postorder(node):
  if node:
    postorder(node.left)
    postorder(node.right)
    print(node.item, end=' ')

# 출력
H D I J E B F K G C A 
```
### 레벨 순회(Level Order Traversal)
![level](https://user-images.githubusercontent.com/81945553/175773871-22b2a03c-b4ce-4dc1-b342-96a3ce7e3c13.png)

> 순서: 레벨별로 왼쪽에서 오른쪽으로      
> 스택 방식을 사용한 재귀적인 코드가 아닌 큐를 이용하여 레벨 별로 순회하는 방법이다.      
> 레벨을 따라 위에서부터 아래로 왼쪽에서 오른쪽으로 탐색한다.

```python
from collections import deque

class Node:
    def __init__(self, item):
        self.item = item
        self.left = None
        self.right = None

def levelorder(root_node):
  queue = deque()         # 큐를 생성 및 초기화
  if root_node:
    queue.append(root_node)    # 처음에는 큐에 루트노드를 삽입한다.
    while(queue):         # 반복문 시작
      node = queue.popleft()     # 큐에서 Dequeue를 진행
      print(node.item, end=' ')
      if node.left:
        queue.append(node.left) # 왼쪽 노드가 있다면 삽입
      if node.right:
        queue.append(node.right) # 오른쪽 노드가 있다면 삽입

# 출력
A B C D E F G H I J K 
```
### 이진 트리의 실제 프로그래밍 활용 사례
* 파일 시스템(디렉터리 구조)
* HTML DOM(Doucument Object Model)
* 표현식 파싱
* 게임 AI(결정 트리)

## 이진 트리 구현

```python
class Node:
  def __init__(self, data):
    self.data = data
    self.left = None
    self.right = None

class BinaryTree:
  def __init__(self):
    self.root = None

  # 레벨 순서로 노드 삽입(왼쪽부터 채움)
  def insert(self, data):
    new_node = Node(data)

    if not self.root:
      self.root = new_node
      return

    # 레벨 순서 탐색을 위한 큐
    queue = [self.root]

    while queue:
      node = queue.pop(0)

      # 왼쪽 자식이 비어있으면 삽입
      if not node.left:
        node.left = new_node
        return
      else:
        queue.append(node.left)

      
```

## 이진 탐색 트리 구현
