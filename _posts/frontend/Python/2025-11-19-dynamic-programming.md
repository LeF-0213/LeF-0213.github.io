---
layout: post
related_posts:
    - /frontend/python
title:  "[Python] Dynamic Programming(동적 프로그래밍)"
date:   2025-11-19
categories:
  - frontend
  - python
description: >
  
---
* toc
{:toc .large-only}

# 동적 프로그래밍(Dynamic Programming)이란?

동적 프로그래밍(Dynamic Programming, DP)은 복잡한 문제를 더 작은 부분 문제들로 나누어 해결하고, 그 결과를 저장했다가 필요할 때 재사용하는 알고리즘 기법이다. 이를 통해 중복된 계산을 피하고 효율성을 극대화한다.

동적 프로그래밍이 효과적으로 작동하기 위해서는 다음 두가지 조건을 만족해야 한다. 동일한 부분 문제가 반복적으로 나타나야 하고, 더 큰 문제의 최적 해가 작은 부분 문제의 최적 해들로부터 구성되어야 한다.

## 분할 정복(Divide&Conquer)과의 차이점