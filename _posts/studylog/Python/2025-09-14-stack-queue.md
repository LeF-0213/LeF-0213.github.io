---
layout: post
related_posts:
    - /studylog/python
title:  "Tree"
date:   2025-09-14
categories:
  - studylog
  - python
description: >
  예시를 통해 
---
* toc
{:toc .large-only}

Tree
비선형 자료구조(나무를 거꾸로 뒤집어 놓은 형태)

단말 노드(leaf node)
더 이상의 자식 노드가 없는 노드

이진 트리를 배열화 시키기
트리를 배열로 표현할 때 배열의 인덱스 0은 사용하지 않는다.

왼쪽 자식 노드 => 부모 노드의 배열 인덱스 x 2
오른쪽 자식 노드 => 부모 노드의 배열 인덱스 x 2 + 1

루트 노드를 배열 인덱스 0으로 하고 왼쪽 자식 노드는 부모 노드의 배열 인덱스 x 2 + 1 
루트 노드를 배열 인덱스 0으로 하고 오른쪽 자식 노드는 부모 노드의 배열 인덱스 x 2 + 2

이진 트리 탐색 방법(순회 방법) => 3개 방법이 존재
1. 전위 현재 노드를 부모 노드로 생각하고 부모노드 -> 왼쪽 자식 노드 -> 오른쪽 자식 노드
2. 중위 현재 노드를 부모 노드로 생각하고 왼쪽 자식 노드 -> 부모 노드 -> 오른쪽 자식 노드
3. 후위 현재 노드를 부모 노드로 생각하고 왼쪽 자식 노드 -> 오른쪽 자식 노드 -> 부모 노드

일반적으로 노드를 "방문"한다
전위순회

3 - 4 - 2 - 8 - 9 - 7 - 1 

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

