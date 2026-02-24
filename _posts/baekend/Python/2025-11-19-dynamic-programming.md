---
layout: post
related_posts:
    - /frontend/python
title:  "[Python] 동적 프로그래밍(Dynamic Programming, DP)"
date:   2025-11-19
categories:
  - frontend
  - python
description: >
  
---
* toc
{:toc .large-only}

# 동적 프로그래밍(Dynamic Programming)이란?

동적 프로그래밍(Dynamic Programming, DP)은 복잡한 문제를 더 작은 부분 문제들로 나누어 해결하고, 그 결과를 **메모이제이션(Memoization)**했다가 필요할 때 재사용하는 알고리즘 기법이다. 
이를 통해 중복된 계산을 피하고 효율성을 극대화한다.

---

## DP가 효율적인 문제의 특징

![dp](https://laboputer.github.io/assets/img/algorithm/algorithm/03_dp_1.PNG)

> **최적 부분 구조**(Optimal Substructure): 
> * 전체 문제의 최적해가 부분 문제의 최적해로부터 구해질 수 있게 분할 가능한 구조  
> * 예: 최단 경로 문제에서 A -> C의 최단 경로는 A -> B 와 B -> C의 최단 경로를 포함

> **중복 부분 문제(Overlapping Subproblems)**
> * 전체 문제를 해결함에 있어 똑같은 부분 문제가 중복되어 발생하는 문제
> * 예: 피보나치 수열에서 F(3)이 여러 번 계산됨

> **추가 특징**:
> * 상태를 명확하게 정의할 수 있어야 한다.
> * 상태 전이 관계를 식으로 표현할 수 있어야 한다.(점화식, 다음 항을 구하기 위해 이전 항의 값을 사용하는 규칙)

## 분할 정복(Divide & Conquer)과의 차이점

![indiffer](https://github.com/user-attachments/assets/bbea21c6-f635-4997-ad81-ebba86e4e7e8)

### 분할 정복 (Divide & Conquer)

> 문제를 **독립적인** 하위 문제들로 분할      
> 각 하위 문제를 **재귀적으로** 해결      
> 하위 문제들이 **중복되지 않음**     
> 예시: 병합 정렬, 퀵 정렬, 이진 탐색

### 동적 프로그래밍(Dynamic Programming)

> 문제를 **중복되는** 하위 문제들로 분할        
> 각 하위 문제의 결과를 **저장(메모이제이션)**        
> 저장된 결과를 **재사용**하여 중복 계산 방지   
> 예시: 피보나치 수열, 배낭 문제, 최장 공통 부분 수열     

**핵심 차이**: 분할 정복은 하위 문제가 독립적이지만, DP는 하위 문제가 중복되어 결과를 재사용한다.

### 분할 정복 vs 동적 프로그램 차이

| 구분 | 분할 정복 (Divide & Conquer) | 동적 프로그래밍 (Dynamic Programming) |
|------|-------------------------------|----------------------------------------|
| **하위 문제** | 독립적 (중복 없음) | 중복됨 |
| **해결 방식** | 재귀적 분할 | 메모이제이션 + 재사용 |
| **시간 복잡도** | O(n log n) ~ O(n²) | O(n) ~ O(n²) (최적화됨) |
| **공간 복잡도** | O(log n) ~ O(n) | O(n) ~ O(n²) (저장 필요) |
| **대표 알고리즘** | 병합정렬, 퀵정렬, 이진탐색 | 피보나치, 배낭문제, LCS |
| **핵심 개념** | 분할 → 정복 → 결합 | 중복 제거 → 최적해 구성 |

---

## 메모이제이션과 탑다운 방식
### 메모이제이션(Memoization)

> 메모이제이션은 '기록하다(memorandum)'에서 유래되어다는 설이 있다. 
> 함수의 결과를 캐시(Cache)에 저장하여, 동일한 입력에 대해 다시 계산하지 않고 저장된 값을 반환하는 기법이다.

#### 목적: 중복 계산 방지

> 알고리즘에서 동일한 입력에 대해 동일한 결과를 반환하는 함수(즉, 부수 효과가 없는 순수 함수)가 **여러 번 호출될 때**, 매번 계산을 새로하는 비효율을 제거하기 위함이다. 
> 특히 재귀함수로 구현된 **탑다운(Top-Down)에서 이 문제가 두드러진다.**

#### 구현 요소
메모이제이션을 구현하는데 필요한 두가지 핵심 요소

| 요소 | 역할 | 구체적 구현 |
|------|-------|--------------|
| **저장소(Cache)** | 이전에 계산된 입력 값과 그 결과를 저장하는 공간이다. | 배열, 리스트, 해시 테이블(딕셔너리) 등이 사용된다. |
| **조회(Lookup)** | 함수가 호출될 때마다, 원하는 결과가 저장소에 이미 있는지 확인하는 과정이다. | 저장소의 인덱스나 키를 통해 확인한다. |

---

### 탑다운 방식(Top-Down Approach)

탑다운 방식은 이름 그래도 가장 **큰 문제**에서 시작하여 해결에 필요한 **작은 하위 문제**로 점차 내려가며 해결하는 방식이다. 주로 **재귀 함수**를 사용하여 구현된다.

#### 핵심 원리: 재귀와 메모이제이션의 결합(탑다운 방식 = 재귀 + 메모이제이션)

> * 재귀(Recursion): 최상위 문제를 풀기 위해, 그보다 작은 하위 문제들을 다시 자기 자신(함수)을 호출하여 해결한다. 이 과정은 가장 기본적인(bse) 하위 문제에 도달할 때까지 반복된다.
> * 메모이제이션(Memoization): 재귀 호출 과정에서 **동일한 하위 문제**가 여러 번 호출되는 것을 방지하기 위해, 계산된 결과를 **저장소(캐시)**에 기록해 둔다.

#### 동작 순서

> 1. **함수 호출**: 가장 큰 문제(예: F(n))를 해결하기 위해 함수가 호출된다.        
> 2. **캐시 확인**: 입력값에 해당하는 결과가 이미 저장소에 있는지 조회한다.          
> 3. **결과 반환(조회 성공 시)**: 결과가 있다면, 새로 계산하지 않고 **저장된 값**을 즉시 반환한다.       
> 4. **새로운 계산(조회 실패 시)**: 결과가 없다면, 재귀 호출을 통해 하위 문제들을 해결한다.      
> 5. **결과 저장 및 반환**: 하위 문제들의 결과를 조합하여 최종 결과를 얻으면, 이 결과를 **저장소에 기록**한 후 반환한다.     

#### 탑다운 방식의 특징 및 장단점

> **장점**
> * **직관적이고 구현이 용이**: 문제 정의를 그대로 재귀 함수 형태로 옮기기 때문에 코드가 논리적으로 이해하기 쉽다.
> * **필요한 하위 문제만 계산**: 문제 해결에 **실제로 필요한 하위 문제**들만 계산하고 나머지는 건너뛰기 때문에, 불필요한 계산을 줄일 수 있다.(특히 하위 문제 공간 전체를 다 계산할 필요가 없을 때 유리하다.)

> **단점**
> * **함수 호출 오버헤드**: 재귀 호출은 반복문보다 함수 호출과 복귀에 따른 메모리 사용 및 시간 오버헤드가 발생할 수 있다. 
> * **스택 오버플로우 위험**: 문제의 크기(n)가 매우 클 경우, 재귀의 깊이가 깊어져 **콜 스택 메모리 크기 제한**을 초과하게 되면, **스택 오버플로우(Stack Overflow)**가 발생하는 위험이 있다.

→ 따라서 탑다운 방식은 복잡한 점화식을 다루거나 계산 순서를 명확히 알기 어려울 때 유용하다.

---

### 보텀업 방식(Bottom-up Approach)

보텀업 방식은 이름 그대로 가장 **작은 하위 문제**부터 시작해서 그 결과를 차례로 쌓아 올리며 최종적으로 **가장 큰 문제**의 해답을 완성하는 방법이다.

#### 핵심 원리: 테이블 채우기(Tabulation)

> * **반복문(Iteration)**: 보텀업 방식은 재귀 호출 대신 주로 **반복문(for 루프)**을 사용하여 구현된다.
> * **테이블(배열) 사용**: 작은 하위 문제들의 해답을 저장하기 위한 **DP 테이블** 또는 **배열**을 정의한다.
> * **순차적 계산**: 가장 작은 입력 값(보통 0또는 1)에 대한 기본 값(Base Case)을 테이블에 먼저 기록하고, 이후 반복문을 통해 입력 크기를 1, 2, 3, ..., n 순서로 증가시키면서 테이블의 값을 순차적으로 채워나간다.

→ 이러한 접근법을 **테이블화(Tabulation)**라고도 부른다.

#### 동작 순서(피보나치 수열 $$F(n)$$ 예시)

> 1. **DP 테이블 초기화**: 크기 n 이상의 배열 $$\mathbf{D}$$를 생성하고, 가장 작은 문제의 해답(Base Case)을 초기화 한다.
>   * D[0] = 0
>   * D[1] = 1
> 2. **반복 계산**: $$\mathit{i}$$ = 2부터 n까지 반복문을 실행한다.
> 3. **점화식 적용**: 각 단계 $$\mathit{i}$$에서, 이미 계산이 완료된 **더 작은 하위 문제**들의 결과($$\mathit{D[i-1]}$$과 $$\mathit{D[i-2]}$$)를 활용하여 현재 문제 $$\mathit{i}$$의 해답을 계산한다.
>   * $$\mathit{D[i] = D[i-1] + D[i-2]}$$
> 4. **최종 결과 반환**: 반복이 끝난 후, 테이블의 마지막 값 $$\mathbf{D[n]}$$이 전체 문제의 최종 해답이 된다.

#### 보텀업 방식의 특징 및 장단점

> **장점**
> * **함수 호출 오버헤드 없음**: 반복문을 사용하므로 재귀 호출에 수반되는 스택 프레임 생성, 복귀 주소 저장 등의 **오버헤드가 발생하지 않는다.** 따라서 일반적으로 탑다운 방식보다 **실행 속도가 빠르다.**
> * **스택 오버플로우 위험 없음**: 재귀를 사용하지 않기 때문에 **스택 오버플로우 위험이 전혀 없다.** $$\mathit{N}$$이 매우 큰 문제에 특히 유리하다.
> * **메모리 최적화 용이**: 때로는 DP 테이블 전체를 저장할 필요 없이, 필요한 이전 값 몇 개만 변수로 저장하여 **메모리 사용량을 줄일 수 있다.** (예: 피보나치 수열에서 $$F(n)$$을 계산할 때 $$F(n-1)$$과 $$F(n-2)$$만 필요하므로, 전체 배열을 저장할 필요가 없다.)

> **단점**
> * **직관성 저하**: 문제를 재귀적으로 정의한 탑다운 방식에 비해, 반복문을 설계하는 과정이 조금 덜 직관적일 수 있다.
> * **모든 하위 문제 계산**: 문제 해결에 실제로 **필요하지 않은** 하위 문제까지도 테이블을 채우기 위해 계산해야 할 수 있다.(탑다운은 필요한 것만 계산한다.)

→ 따라서 보텀업 방식은 동적 프로그래밍 문제에서 **성능 최적화와 메모리 안정성**이 중요할 때 선호되는 구현 전략이다.

---

## DP 예제 모음

- [피보나치 수열](#피보나치-수열)
- [배낭 문제 (0/1 Knapsack)](#배낭-문제-01-Knapsack)

### 실행 시간 측정 데코레이터

```python
import time
from functools import wraps

def measure_time(func):
  @wraps(func)
  def wrapper(*args, **kwargs):
    start = time.time()
    result = func(*args, **kwargs)
    end = time.time()
    print(f"실행 시간: {(end - start) * 1000:.4f}ms")
    return result
  return wrapper
```
### 피보나치 수열

```python
print("=" * 60)
print("문제 1: 피보나치 수열(n번째 피보나치 수 구하기)")
print("=" * 60)

# 일반 재귀 - O(2^n) 시간복잡도 (비효율적 - 비교용)
@measure_time
def fibonacci_recursive(n):
  if n <= 1:
    return n
  return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2)

# 탑다운 방식 - O(n) 시간복잡도 (메모이제이션)
@measure_time
def fibonacci_topdown(n, memo=None):
  if memo is None:
    memo = {}

  if n in memo:
    return memo[n]

  if n <= 1:
    return n

  memo[n] = fibonacci_topdown(n - 1, memo) + fibonacci_topdown(n - 2, memo)
  return memo[n]

# 바텀업 방식 - O(n) 시간복잡도
@measure_time
def fibonacci_bottomup(n): 
  if n <= 1:
    return n

  dp = [0] * (n + 1)
  dp[1] = 1

  for i in range(2, n + 1):
    dp[i] = dp[i - 1] + dp[i - 2]

  return dp[n]

# 테스트 결과
n = 35
print(f"[피보나치 수열 F({n}) 계산 결과]")
print("일반 재귀: ")
result1 = fibonacci_recursive(n)
print(f"결과: {result1}")  # 재귀 호출이 1억번이상 실행됨

print("탑다운: ")
result2 = fibonacci_topdown(n)
print(f"결과: {result2}")  

"""
탑다운: 
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0520ms
실행 시간: 0.0000ms
실행 시간: 0.0567ms
실행 시간: 0.0010ms
실행 시간: 0.0620ms
실행 시간: 0.0010ms
실행 시간: 0.0646ms
실행 시간: 0.0000ms
실행 시간: 0.0701ms
실행 시간: 0.0000ms
실행 시간: 0.0741ms
실행 시간: 0.0000ms
실행 시간: 0.0780ms
실행 시간: 0.0000ms
실행 시간: 0.0820ms
실행 시간: 0.0000ms
실행 시간: 0.0861ms
실행 시간: 0.0000ms
실행 시간: 0.0899ms
실행 시간: 0.0010ms
실행 시간: 0.0939ms
실행 시간: 0.0000ms
실행 시간: 0.0992ms
실행 시간: 0.0000ms
실행 시간: 0.1030ms
실행 시간: 0.0000ms
실행 시간: 0.1070ms
실행 시간: 0.0000ms
실행 시간: 0.1099ms
실행 시간: 0.0000ms
실행 시간: 0.1140ms
실행 시간: 0.0000ms
실행 시간: 0.1180ms
실행 시간: 0.0000ms
실행 시간: 0.1218ms
실행 시간: 0.0000ms
실행 시간: 0.1268ms
실행 시간: 0.0000ms
실행 시간: 0.1311ms
실행 시간: 0.0000ms
실행 시간: 0.1338ms
실행 시간: 0.0000ms
실행 시간: 0.1390ms
실행 시간: 0.0000ms
실행 시간: 0.1421ms
실행 시간: 0.0000ms
실행 시간: 0.1459ms
실행 시간: 0.0000ms
실행 시간: 0.1500ms
실행 시간: 0.0000ms
실행 시간: 0.1543ms
실행 시간: 0.0000ms
실행 시간: 0.1581ms
실행 시간: 0.0000ms
실행 시간: 0.1612ms
실행 시간: 0.0000ms
실행 시간: 0.1652ms
실행 시간: 0.0000ms
실행 시간: 0.1681ms
실행 시간: 0.0000ms
실행 시간: 0.1731ms
실행 시간: 0.0012ms
실행 시간: 0.1762ms
실행 시간: 0.0000ms
실행 시간: 0.1800ms
실행 시간: 0.0000ms
실행 시간: 0.1860ms
결과: 9227465
"""

print("바텀업: ")
result3 = fibonacci_bottomup(n)
print(f"결과: {result3}")

"""
바텀업: 
실행 시간: 0.0050ms
결과: 9227465
"""
```
### 배낭 문제 (0/1 Knapsack)

```python
print("=" * 60)
print("문제 3: 0/1 배낭 문제 (최대 가치 구하기)")
print("각 물건을 넣거나 넣지 않거나 선택하여 ")
print("배낭의 용량 내에서 최대 가치를 구하기")
print("=" * 60)

# 탑다운 방식 - O(n*W) 시간복잡도
@measure_time
def knapsack_topdown(weights, values, capacity, n=None, memo=None):
  if n is None:
    n = len(weights)
  if memo is None:
    memo = {}

  # 기저 사례(Base Case, 더 이상 쪼갤 수 없는 마지막 단계, 재귀 호출이 멈추는 조건)
  if n == 0 or capacity == 0:
    return 0

  # 메모이제이션 확인
  if (n, capacity) in memo:
    return memo[n, capacity]

  # 현재 물건을 넣을 수 없는 경우
  if weights[n - 1] > capacity:
    result = knapsack_topdown(weights, values, capacity, n - 1, memo)
  else:
    # 현재 물건을 넣는 경우와 넣지 않는 경우 중 최대값
    include = values[n - 1] + knapsack_topdown(weights, values, capacity - weights[n - 1], n - 1, memo)
    exclude = knapsack_topdown(weights, values, capacity, n - 1, memo)
    result = max(include, exclude)

  memo[(n, capacity)] = result
  return result

# 바텀업 방식 - O(n*W) 시간복잡도
@measure_time
def knapsack_bottomup(weights, values, capacity):
  n = len(weights)
  dp = [[0] * (capacity + 1) for _ in range(n + 1)]

  for i in range(1, n + 1):
    for w in range(capacity + 1):
      # 현재 물건을 넣을 수 없는 경우
      if weights[i - 1] > w:
        dp[i][w] = dp[i - 1][w]
      else:
        # 넣는 경우와 넣지 않는 경우 중 최댓값
        include = values[i - 1] + dp[i - 1][w - weights[i - 1]]
        exclude = dp[i - 1][w]
        dp[i][w] = max(include, exclude)

  # 바텀업 + 선택한 물건 추적
  selected_items = []
  w = capacity
  for i in range(n, 0, -1):
    if dp[i][w] != dp[i - 1][w]:
      selected_items.append(i - 1)
      w -= weights[i - 1]

  return dp[n][capacity], selected_items[::-1]

# 배낭 문제 테스트
weights = [2, 3 , 4, 5, 6]
values = [3, 4, 5, 8, 10]
capacity = 10

print("[배낭 문제 설정]")
print("물건 정보: ")
for i in range(len(weights)):
  print(f"물건 {i + 1}: 무게={weights[i]}kg, 가치={values[i]}만원")
print(f"배낭 용량: {capacity}kg")

"""
물건 정보: 
물건 1: 무게=2kg, 가치=3만원
물건 2: 무게=3kg, 가치=4만원
물건 3: 무게=4kg, 가치=5만원
물건 4: 무게=5kg, 가치=8만원
물건 5: 무게=6kg, 가치=10만원
배낭 용량: 10kg
"""

print("탑다운: ")
result = knapsack_topdown(weights, values, capacity)
print(f"최대 가치: {result}만원")

"""
탑다운: 
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0041ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0041ms
실행 시간: 0.0150ms
실행 시간: 0.0701ms
실행 시간: 0.0732ms
실행 시간: 0.0010ms
실행 시간: 0.0031ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0043ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0031ms
실행 시간: 0.0112ms
실행 시간: 0.0172ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0038ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0031ms
실행 시간: 0.0110ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0043ms
실행 시간: 0.0000ms
실행 시간: 0.0000ms
실행 시간: 0.0041ms
실행 시간: 0.0112ms
실행 시간: 0.0260ms
실행 시간: 0.0460ms
실행 시간: 0.1242ms
최대 가치: 15만원
"""

print("바텀업: ")
max_value, selected = knapsack_bottomup(weights, values, capacity)
print(f"최대 가치: {max_value}만원")
print(f"선택한 물건: {[i + 1 for i in selected]}")
total_weight = sum(weights[i] for i in selected)
print(f"총 무게: {total_weight}kg / {capacity}kg")

"""
바텀업: 
실행 시간: 0.0141ms
최대 가치: 15만원
선택한 물건: [1, 2, 4]
총 무게: 10kg / 10kg
"""
```
