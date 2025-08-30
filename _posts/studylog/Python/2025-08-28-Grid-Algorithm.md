---
layout: post
related_posts:
    - /studylog/python
title:  "그리드 알고리즘"
date:   2025-08-28
categories:
  - studylog
  - python
  - algorithm
description: >
  
---
* toc
{:toc .large-only}

## 그리드(탐욕 알고리즘)
결정을 할때 눈 앞에 보이는 것이 최선의 선택(반복 X)
이 결정이 정답이다라는 확신은 가지지 않는다

전통적인 거스름돈 내어주기
손님에게 8원을 거슬러 줘야 한다. 동전의 개수가 5, 4, 1원만 존재
가장 동전의 개수를 적게 만들기 위한 방법은
1. 5원 -> 1개, 1원 -> 3개
2. 4원 -> 2개
3. 1원 -> 8개

가장 큰 값의 동전을 먼저 고르는 전력을 사용한다. => 특성
그리드 알고리즘이 항상 정답을 알려준다고는 보장하지 못한다.

그리드 알고리즘의 특성상 최적의 해를 보장하고, 이를 활용하여 문제 해결하는 방법이 존재한다,
- 최적 부분 구조, 그리드 선택 속성

![](https://velog.velcdn.com/images/dankj1991/post/22a64eb0-d5fa-477d-b952-e9d306a86010/image.png)
### 신장 트리
모든 정점(느디)이 간선으로 연결되어있고, 간선의 개수가 노드의 개수보다 하나 적은 그래프(모든 노드를 지나칠 수 있다)

### 최소 신장 트리
그리드 알고리즘을 사용하는 대표적인 트리 형태의 자료구조
(최소 + 신장 트리)
최소 신장 트리
가중치의 합이 최소이면 최소 신장 트리라고 이야기 한다.
(경우에 따라서 결과가 한 개 이상 나올 수 있다)

구하는 대표적인 알고리즘
1. 프림 알고리즘
2. 크루스칼 알고리즘

### 크루스칼 알고리즘
1. 모든 간선의 가중치 값 기준으로 오름차순 정렬
2. 가중치가 낮은 간선부터 최소 신장 트리에 하나씩 추가(그리드 방식의 선택) 단, 순한을 형성하면 (X)
3. 2번 과정을 만족할 때까지 반복(신장 트리)

![](https://velog.velcdn.com/images/dankj1991/post/a4261545-98dc-4f12-990a-6c68404334f5/image.png)

### 프림 알고리즘
1. 노드값 중에서 시작 노드는 값이 가장 작은 노드로부터 출발

![](https://velog.velcdn.com/images/dankj1991/post/03b9e88c-a82c-4dc5-8801-f767c282c776/image.png)

거스름돈을 돌려받을 때 최소한의 화폐수로 받기를 원한다.
거스름돈은 amount가 있을 때 화폐 단위는 [1, 10, 50, 100]을 최소한으로 사용된 화폐 리스트르르 반환하는 함수를 작성하시오

amount = 123 => [100, 10, 10, 1, 1, 1]

```python
unit = [1, 10, 50, 100]
amount = 123

def solution(amount, unit):
    answer = []
    unit.sort(reverse = True)
    
    for i in unit:
        cnt = amount // i
        answer.extend([i] * cnt)
        amount %= i

    return answer
    
print(solution(amount, unit))
```

