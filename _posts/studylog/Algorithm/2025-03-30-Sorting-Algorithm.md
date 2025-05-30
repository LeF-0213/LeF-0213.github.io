---
layout: post
related_posts:
  - /studylog/algorithm
title:  "정렬 알고리즘"
date:   2025-03-22
categories:
  - studylog
  - algorithm
description: >
  정렬 알고리즘(삽입 정렬, 병합 정렬, Heap 정렬)
---
* toc
{:toc .large-only}

## 정렬에서의 키란?
정렬되지 않은 영역의 맨 앞에 있는 것을 의미

# 정렬
사용자가 정의한 순서대로 데이터를 나열하는 방법
(오름차순 혹은 내림차순으로 출력 가능)

# 삽입 정렬
데이터 영역에서 정렬된 영역과 되지 않은 영역을 나누어 분리

## 삽입 정렬 방법
1. 정렬된 영역과 정렬되지 않은 영역으로 분류
2. 키와 정렬된 영역의 맨 끝 값부터 거슬러 올라가며 정렬
3. 2번을 반복하여 데이터가 정렬될 때까지 반복

## 삽입 정렬의 핵심
* **최선의 경우**: 시간복잡도`O(N)`
* **최악의 경우**: 시간복잡도`O(N^2)`

# 병합 정렬(Merge Sort)
정렬되지 않은 영역을 쪼개서 각각의 영역을 정렬하고 이를 합치는 정렬 방식(`분활 정복`)
![MergeSort](https://gmlwjd9405.github.io/images/algorithm-merge-sort/merge-sort-concepts.png)

## 병합 정렬의 핵심
병합할 때 부분 정렬하는 부분을 어떻게 구현하는가?

* (`포인터` => C언어의 포인터를 의미하는 것은 아님)
1. 포인터가 데이터의 맨 처음 데이터인 1과 2를 가리킨다.
2. 포인터 중 더 작은 것을 비교한다.

# Heap 정렬
tree를 이용하는 정렬

# 위상 정렬(+ 진입 차수)
진입 차수