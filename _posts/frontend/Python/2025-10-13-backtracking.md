---
layout: post
related_posts:
    - /frontend/python
title:  "백트래킹 알고리즘"
date:   2025-10-13
categories:
  - frontend
  - python
description: >
  예시를 통해 
---
* toc
{:toc .large-only}

# 백트래킹(Backtracking)알고리즘이란?
가능성이 없는 곳에서는 되돌아가고, 가능성이 있는 곳을 탐색하는 알고리즘
![Backtracking](https://goldenrabbit.co.kr/wp-content/uploads/2023/12/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7-2023-12-29-%EC%98%A4%EC%A0%84-11.09.45.png)
완전 탐색하는 방법보다 효율적이다.

## 유망함수(Promising Function)란?
백트래킹 알고리즘에서 특정 상태를 탐색할 가치가 있는지 판단하는 함수(즉, 해가 될 가능성 판단)
1. 유효한 해의 집합을 정의한다.
2. 위 단계에서 정의한 집합을 그래프로 표현한다.
3. 유망 함수를 정의한다.
4. 백트래킹 알고리즘을 활용해서 해를 찾는다.

### 예시 - 합이 6을 넘는 경우
```txt
{1, 2, 3, 4} 중 2개의 숫자를 뽑아서 합이 6을 초과하는 경우(뽑는 순서가 다르면 다른 경우의 수로 간주한다.)
```

1. 유효한 해의 집합을 정의
![BacktrackingExImg1](https://goldenrabbit.co.kr/wp-content/uploads/2023/12/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7-2023-12-29-%EC%98%A4%EC%A0%84-11.11.25.png)

2. 정의한 해의 집합을 그래프로 표현
![BacktrackingExImg2](https://goldenrabbit.co.kr/wp-content/uploads/2023/12/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7-2023-12-29-%EC%98%A4%EC%A0%84-11.11.42-1536x543.png)

3. 유망 함수를 정의
* 그래프에서 백트래킹을 수행한다. 
* '처음에 뽑은 숫자가 3미만이면 백트래킹한다' 라는 특정 조건을 정의
* 이를 유망 함수를 정의한다고 한다.
* 1, 2는 유망 함수와는 상관없이 깊이 우선 탐색 알고리즘에 의해 방문하지만 답이 아니므로 백트래킹한다.
![BacktrackingExImg3](https://goldenrabbit.co.kr/wp-content/uploads/2023/12/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7-2023-12-29-%EC%98%A4%EC%A0%84-11.12.40-1536x532.png)

4. 백트래킹 알고리즘을 활용해서 해를 찾기
* 3은 유망함수를 통과한다. 그 이후 3 -> 4로 가서 답을 찾는다.
![BacktrackingExImg4](https://goldenrabbit.co.kr/wp-content/uploads/2023/12/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7-2023-12-29-%EC%98%A4%EC%A0%84-11.13.07-1536x566.png)

5. 같은 방식으로 나머지도 탐색을 진행
![BacktrackingExImg5](https://goldenrabbit.co.kr/wp-content/uploads/2023/12/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7-2023-12-29-%EC%98%A4%EC%A0%84-11.13.21-1536x536.png)