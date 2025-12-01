---
layout: post
related_posts:
    - /frontend/python
title:  "[Python] 클로저와 데코레이터(Closure&Decorator)"
date:   2025-10-28
categories:
  - frontend
  - python
description: >
  예제를 통해 클로저와 데코레이터에 대한 이해
---
* toc
{:toc .large-only}

# 클로저(Closure)란? [참조](https://wikidocs.net/134789)

* 클로저란 **외부 함수의 변수를 기억하는 내부 함수**를 말한다.
* 간단히 말해 함수 안에 내부 함수(inner function)를 구현하고 그 내부 함수를 반환하는 함수이다.
* 이때 외부 함수는 자신이 가진 변숫값 등을 내부 함수에 전달하여 실행되도록 한다.

## 기본 예제
> 어떤 수에 항상 3을 곱해 반환하는 함수를 만들어보면

```python
def mul3(n):
  return n * 3
```
> 어떤 수에 항상 5를 곱해 반환하는 함수를 만들어보면

```python
def mul5(n):
  return n * 5
```
> 하지만 이렇게 필요할 때마다 mul6(), mul7(), mul8(),... 과 같은 함수를 만드는 것은 비효율적이다. 이 문제를 효율적으로 해결하려면 다음과 같이 클래스를 사용하면 된다.

```python
class Mul:
  def __init__(self, m):
    self.m = m

  def mul(self, n):
    return self.m * n

if __name__=="__main__":
  mul3 = Mul(3)
  mul5 = Mul(5)

  print(mul3.mul(10)) # 30 출력
  print(mul5.mul(10)) # 50 출력
```
> 클래스를 이용하면 특정 값을 미리 설정하고, 그 다음부터 mul() 메서드를 사용하면 원하는 형태로 호출할 수 있다.        
> `__call__` 메서드를 이용하여 다음과 같이 개선할 수도 있다.

```python
class Mul:
  def __init__(self, m):
    self.m = m

  def __call__(self, n):
    return self.m * n

if __name__ == "__main__":
  mul3 = Mul(3)
  mul5 = Mul(5)

  print(mul3(10))  # 30
  print(mul5(10))  # 50
```
> mul() 함수 이름을 `__call__`로 바꾸었다.    
> `__call__`함수는 Mul 클래스로 만든 객체에 인수를 전달하여 바로 호출할 수 있도록 하는 메서드이다.    
> `__call__`메서드를 이용하면 예제처럼 mul3객체를 **mul3(10)**처럼 호출할 수 있다.    
> 이렇게 클래스로 만드는 방법이 일반적이긴 하지만, 더 간편한 방법이 있다.   

```python
def mul(m):
  def wrapper(n):
    return m * n
  return wrapper

if __name__ == "__main__":
  mul3 = mul(3)
  mul5 = mul(5)

  print(mul3(10))  # 30
  print(mul5(10))  # 50
```
> `mul(3)`을 호출하면 외부 함수 mul은 종료되지만,   
> 반환된 wrapper 함수는 여전히 `m = 3`값을 기억하고 있다.   
> 이후 `mul3(10)`을 호출하면 wrapper 함수가 기억하고 있던 `m = 3`값을 사용해서 `3 * 10 = 30`을 계산한다.

> 즉, 클로저는 **함수가 생성될 때의 환경(변수 값)을 기억하는 특별한 함수**이다.     
> 반환된 wrapper함수가 바로 클로저이고, mul과 같이 클로저를 만들어내는 함수를 클로저 팩토리(closure factory)함수라고 한다.

### 클로저의 장점

* **데이터의 은닉**: 외부에서 직접 접근할 수 없는 private 변수 생성
* **상태 유지**: 함수 호출 간에 상태를 유지
* **팩토리 패턴**: 설정에 따라 다른 동작을 하는 함수 생성

# 데코레이터(Decorator)란?

* 데코레이터는 **기존 함수를 수정하지 않고 기능을 추가하는 함수**이다. 
* `@` 기호를 사용하여 간편하게 적용할 수 있다.
* 다른 함수를 인자로 받아서 감싸는 방식으로 작동한다.

## 기본 예제

> "함수가 실행됩니다."라는 문자열을 출력하는 myfunc함수를 준비한다.

```python
def myfunc():
  print("함수가 실행됩니다.")
```
> 함수 실행 시간을 측정하기로 한다. 함수 실행 시간은 함수가 시작하는 순간의 시간과 함수가 종료되는 순간의 시간차이를 구하면 알 수 있다.

```python
import time

def myfunc():
  start = time.time()
  print("함수가 실행됩니다.")
  end = time.time()
  print(f"함수 수행시간: {end - start}")

myfunc()
```
> 하지만 실행 시간을 측정해야 하는 함수가 myfunc 말고도 많다면 이런 코드를 모든 함수에 적용하는 것은 너무 비효율적이다.     
> 이때 클로저를 이용하면 효율적인 방법을 찾을 수 있다.

```python
import time

def elapsed(original_func):   # 기존 함수를 인수로 받는다.
  def wrapper():
    start = time.time()
    result = original_func()  # 기존 함수를 수행한다.
    end = time.time()
    print(f"함수 수행시간: {end - start}") # 기존 함수의 수행시간을 출력한다.
    return result   # 기존 함수의 수행 결과를 반환한다.
  return wrapper

def myfunc():
  print("함수가 실행됩니다.")

decorated_myfunc = elapsed(myfunc)
decorated_myfunc()

# 결과
함수가 실행됩니다.
함수 수행시간: 0.044229984283447266
```
> elapsed 함수로 클로저를 만들었다. elapsed 함수는 함수를 인수로 받는다.    
> 파이썬은 함수도 객체이므로 함수 자체를 인수로 전달할 수 있다.

> `decorate_myfunc = elapsed(myfunc)`로 생성한 decorated_myfunc를 decorated_myfunc()로 실행하면 
> 실제로는 elapsed 함수 내부의 wrapper 함수가 실행되고, 
> 이 함수는 전달받은 myfunc 함수를 실행하면서 실행시간을 함께 출력한다.

> 클로저를 이용하면 기존 함수에 기능을 덧붙이기가 매우 편리하다.      
> 이렇게 기존 함수를 바꾸지 않고 기능을 추가할 수 있게 만드는 **elapsed 함수**와 같은 클로저를 데코레이터(decorator)라고 한다.

```python
import time

def elapsed(original_func):
  def wrapper():
    start = time.time()
    result = original_func()
    end = time.time()
    print(f"함수 수행시간: {end - start}")
    return result
  return wrapper

@elapsed
def myfunc():
  print("함수가 실행됩니다.")

# decorated_myfunc = elapsed(myfunc)  # @elapsed 데코레이터로 인해 더이상 필요하지 않다.
# decorated_myfunc()

myfunc()

# 결과
myfunc()함수가 실행됩니다.
함수 수행시간: 0.044229984283447266
```
> 파이썬은 함수 위에 `@+함수명`이 있으면 데코레이터 함수로 인식한다.

> myfunc 함수를 매개변수가 있는 함수로 변경하면

```python
import time

def elapsed(original_func):
  def wrapper():
    start = time.time()
    result = original_func()
    end = time.time()
    print(f"함수 수행시간: {end - start}")
    return result
  return wrapper

@elapsed
def myfunc(msg):
  print(f"{msg}을 출력합니다.")

myfunc("You need Python")

# 결과
Traceback (most recent call last):
  File "<pyshell#15>", line 1, in <module>
    myfunc("You need Python")
TypeError: elapsed.<locals>.wrapper() takes 0 positional arguments but 1 was given
```
> 오류 발생 이유:     
> myfunc 함수는 입력 인수가 필요하지만, elapsed 함수 안의 wrapper 함수는 전달받은 myfunc 함수를 입력 인수 없이 호출해 오류가 발생하는 것이다.     
> 그러므로 데코레이터 함수는 기존 함수의 입력 인수에 상관없이 동작하도록 만들어야 한다.      
> 전달받아야 하는 기존 함수의 입력 인수를 알 수 없는 경우에는 `*args`와 `**kwargs` 매개변수를 이용하면 된다.

```python
import time

def elapsed(original_func):
  def wrapper(*args, **kwargs):
    start = time.time()
    result = original_func(*args, **kwargs)
    end = time.time()
    print(f"함수 수행시간: {end - start}")
    return result
  return wrapper

@elapsed
def myfunc(msg):
  print(f"{msg}를 출력합니다.")

myfunc("You need python")

# 결과
You need python를 출력합니다.
함수 수행시간: 0.06546306610107422
```

### 예제 - 인증 체크

```python
def require_auth(func):
  def wrapper(user, *args, **kwargs):
    if user.get('is_authenticated'):
      return func(user, *args, **kwargs)
    else:
      return "인증이 필요합니다."
    return wrapper

@require_auth
def delete_account(user):
  return f"{user['name']}의 계정이 삭제되었습니다."

user1 = {'name': 'Alice', 'is_authenticated': True}
user2 = {'name': 'Bob', 'is_authenticated': False}

# 결과
print(delete_account(user1))  # Alice의 계정이 삭제되었습니다
print(delete_account(user2))  # 인증이 필요합니다
```