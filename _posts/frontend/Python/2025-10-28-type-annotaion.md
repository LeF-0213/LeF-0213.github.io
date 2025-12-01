---
layout: post
related_posts:
    - /frontend/python
title:  "[Python] 타입 어노테이션"
date:   2025-10-28
categories:
  - frontend
  - python
description: >
  
---
* toc
{:toc .large-only}

# 타입 어노테이션(Type Annotation)이란?

* Python 3.5+에서 도입된 **타입 힌트(Type Hint)기능**
* 변수, 함수의 매개변수, 반환값의 **예상 타입을 명시**한다.
* **실행에는 영향을 주지 않는다.**(힌트일 뿐, 강제하지 않음)
* 하지만 코드의 가독성과 정적 분석 도구(myPy 등) 사용, IDE의 자동완성에 도움이 되기 때문에 타입 어노테이션을 사용하는 것이 좋다.

### 기본 문법

```python
# 변수 타입 어노테이션
볌수명: 타입 = 값

# 함수 타입 어노테이션
def 함수명(매개변수: 타입) -> 반환타입:
  return 값
```

## 기본 타입 어노테이션
### 기본 자료형

```python
# 문자열
name: str = '김사과'

# 정수
age: int = 10

# 실수
height: float = 160.5

# 불린
is_lover: bool = True

# None
reuslt: None = None
```
### 함수의 타입 어노테이션

```python
def add(a: int, b: int) -> int:
  return a + b

print(add(10, 3))
print(add('십', '삼'))

# 타입 어노테이션을 확인
add.__annotations__

# 결과
13
십삼
{'a': int, 'b': int, 'return': int}
```

```python
def greet(user: str | None) -> str:
  return f"Hello, {user or 'Guest'}!"

print(greet('김사과'))
print(greet(None))

# 결과
Hello, 김사과
Hello, Guest  
```

## 컬렉션 타입
### List(리스트)

```python
# 문자열 리스트
names: list[str] = ['김사과', '오렌지', '반하나']
# 혼용 리스트
values: list[int | str] = [1, "two", 3, "four"]
# 중첩 리스트
matrix: list[list[int]] = [[1, 2], [3, 4]]

def get_scores() -> list[int]:
  return [90, 85, 88, 92]
```
### Tuple(튜플)

```python
# 고정 길이 튜플(각 위치마자 타입 지정)
point: tuple[int, int] = (10, 20)
person: tuple[str, int, bool] = ("Alice", 25, True)
# 가변 길이 튜플(모든 요소가 같은 타입)
numbers: tuple[int, ...] = (1, 2, 3, 4, 5)

def get_coordinates() -> tuple[float, float]:
  return (37.5665, 126.9780)
```
### Dict(딕셔너리)

```python
# 키: 문자열, 값: 정수
scores: dict[str, int] = {'Alice': 90, 'Bob': 85}

# 키: 정수, 값: 문자열 리스트
student_classes: dict[int, list[str]] = {
  1: ["Math", "English"],
  2: ["Science", "History"]
}

def get_user_info() -> dict[str, str]:
  return {"name": "Alice", "email": "alice@example.com"}
```
### Set(집합)

```python
# 정수 집합
unique_numbers: set[int] = {1, 2, 3, 4, 5}

# 문자열 집합
tags: set[str] = {"Python", "programming", "tutorial"}

def get_unique_ids() -> set[int]:
  return {101, 102, 103}
```
## 고급 타입 어노테이션
### Optional(선택적 타입)
**Python 3.10 전**

```python
from typing import Optional
# 값이 있거나 None일 수 있음
# Optional[int] = int | None
age: Optional[int] = None
name: Optional[str] = "Alice"

def find_user(user_id: int) -> Optional[str]:
  users = {1: "Alice", 2: "Bob"}
  return users.get(user_id) # None 가능

# 결과
print(find_user(1)) # Alice
print(find_user(2)) # Bob
print(find_user(3)) # None
```
**Python 3.10+**

```python
def Hello(name: str | None) -> str:
  if name is None:
    return "Hello, Guest!"
  return f"Hello, {name}!"

# 결과
print(greet("apple")) # Hello, apple
print(greet(None))    # Hello, Guest
```

```python
def find_user(user_id: int) -> str | None:
  if user_id == 1:
    return "apple"
  return None

# 결과
print(find_user(1))   # apple
print(find_user(99))  # None
```

```python
users: dict[int, str] = {
  1: 'apple',
  2: 'banana'
}

def get_suer(user_id: int) -> str | None:
  return users.get(user_id)

# 결과
print(get_user(1))  # apple
print(get_user(2))  # banana
print(get_user(3))  # None
```
### Any(모든 타입 허용)

```python
from typing import Any

# 어떤 타입이든 가능
data: Any = 123
data = "hello"
data = [1, 2, 3]

# 타입을 특정할 수 없을 때
def process(value: Any) -> Any:
  return value
```
### Callable(함수 타입)

```python
from typing import Callable

# 매개변수 (int, int), 반환값 int인 함수
operation: Callable[[int, int], int] = lambda x, y: x + y

# 함수를 매개변수로 받음
def apply_operation(x: int, y: int, func: Callable[[int, int], int]) -> int:
  return func(x, y)

def add(a: int, b: int) -> int:
  return a + b

result = apply_operation(5, 3, add) # 8
```
### Literal(특정 값만 허용)

```python
from typing import Literal

# 'read', 'write', 'execute' 중 하나만 가능
mode: Literal['read', 'write', 'execute'] = 'read'

def open_file(filename: str, mode: Literal['r', 'w', 'a']) -> None:
  pass
```
## 제네릭(Generic)타입 (Python 3.12+ 간소화)
### Python 3.12+ 방식

```python
# 제네릭 함수(간단한 문법)
def first[T](items: list[T]) -> T:
  return items[0]

num = first([1, 2, 3])
text = first(["a", "b"])

# 제네릭 클래스
class Stack[T]:
  def __init__(self) -> None:
    self.items: list[T] = []

  def push(self, item: T) -> None:
    self.items.append(item)

  def pop(self) -> T:
    return self.items.pop()
```
### 복합 타입

```python
data: dict[str, list[tuple[int, str]]] = {
  "scores": [(90, "A"), (85, "B")]
}
```
## 클래스 타입 어노테이션

```python
class Person:
  def __init__(self, name: str, age: int) -> None:
    self.name: str = name
    self.age: int = age

  def greet(self) -> str:
    return f'Hello, I'm {self.name}'

  def is_adult(self) -> bool:
    return self.age >= 18

# 클래스 인스턴스 타입
person: Person = Person("Alice", 25)

def create_person(name: str, age: int) -> Person:
  return Person(name, age)
```
## TypedDict

```python
from typing import TypedDict, NotRequired # Python 3.11+

class UserResponse(TypedDict):
  id: int
  name: str
  email: str
  age: NotRequired[int] # 선택적 필드

# API에서 사용자 정보 가져오기
def fetch_user(user_id: int) -> UserResponse:
  return {
    "id": user_id,
    "name": "Alice",
    "email": "alice@example.com"
  }

def process_user(user: UserResponse) -> str:
  age_info = f", {user['age']}세" if 'age' in user else ""
  return f"{user['name']} ({user['email']}){age_info}"

print(fetch_user(1)) # {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'}
print(process_user(fetch_user(1))) # Alice (alice@example.com)
```
## 특수 타입

```python
from typing import Sequence, Iterable, Mapping

# Sequence(리스트, 튜플 등)
def sum_all(numbers: Sequence[int]) -> int:
  return sum(numbers)

sum_all([1, 2, 3])
sum_all((1, 2, 3))

# Iterable(반복 가능한 객체)
def print_all(items: Iterable[str]) -> None:
  for item in items:
    print(item)

# Mapping(딕셔너리 등)
def get_value(data: Mapping[str, int], key: str) -> int:
  return data.get(key, 0)
```
## 실전 예시
### 예시 - 학생 관리 시스템

```python
class Student:
  def __init__(self, name: str, student_id: int, grades: list[int]) -> None:
    self.name: str = name
    self.student_id: int = student_id
    self.grades: list[int] = grades

  def average_grade(self) -> float:
    if not self.grades:
      return 0.0
    return sum(self.grades) / len(self.grades)

def find_student(
  students: list[Student],
  student_id : int
) -> Student | None:
  for student in students:
    if student.student_id == student_id:
      return student
    return None

def get_top_students(
  students: list[Student],
  count: int = 3
) -> list[tuple[str, float]]:
  sorted_students = sorted(
    students,
    key=lambda s: s.average_grade(),
    reverse=True
  )
  return [
    (s.name, s.average_grade())
    for s in sorted_students[:count]
  ]
```
### 예시 - 데이터 처리

```python
from typing import Callable, Any

def filter_data(
  data: list[dict[str, Any]],
  condition: callable[[dict[str, Any]], bool]
) -> list[dict[str, Any]]:
  return [item for item in data if condition(item)]

def transform_data(
  data: list[dict[str, Any]],
  transformer: Callable[[dict[str, Any]], dict[str, Any]]
) -> list[dict[str, Any]]:
  return [transformer(item) for item in data]

users = [
  "name": "Alice", "age": 25, "score": 90},
  {"name": "Bob", "age": 30, "score": 85}
]

adults = filter_dat(users, lambda u: u['age'] >= 25)

boosted = transform_data(
  users,
  # 사용자 업데이트
  lambda u: {**u, "score": u["score"] + 10}
)
```
## 타입 별칭(TypeAlias)

```python
from typing import TypeAlias

# 타입 별칭
UserId: TypeAlias = int
UserData: TypeAlias = dict[str, str | int]
Matrix: TypeAlias = list[list[int]]

def get_user(user_id: UserId) -> UserData:
  return {"id": user_id, "name": "Alice"}

def process_matrix(matrix: Matrix) -> int:
  return sum(sum(row) for row in matrix)
```
## 타입 어노테이션 정리표(Python 3.10+)

| 타입 | 설명 | 예시 |
|------|------|------|
| `int`, `str`, `float`, `bool` | 기본 타입 | `age: int = 25` |
| `list[T]` | 리스트 | `numbers: list[int] = [1, 2]` |
| `dict[K, V]` | 딕셔너리 | `scores: dict[str, int]` |
| `tuple[T, ...]` | 튜플 | `point: tuple[int, int]` |
| `set[T]` | 집합 | `tags: set[str]` |
| `T \| None` | T 또는 None | `name: str \| None` |
| `T1 \| T2` | T1 또는 T2 | `id: int \| str` |
| `Any` | 모든 타입 | `data: Any` |
| `Callable[[T1, T2], R]` | 함수 타입 | `func: Callable[[int], str]` |
| `Literal['a', 'b']` | 특정 값만 | `mode: Literal['r', 'w']` |
| `Sequence[T]` | 시퀀스 | `items: Sequence[int]` |
| `Iterable[T]` | 반복 가능 | `items: Iterable[str]` |
| `Mapping[K, V]` | 매핑 | `data: Mapping[str, int]` |

