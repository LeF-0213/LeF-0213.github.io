---
layout: post
related_posts:
    - /frontend/python
title:  "가비지 컬렉션과 Python의 자동 메모리 관리 시스템"
date:   2025-10-21
categories:
  - frontend
  - python
description: >
  가비지 컬렉션 과정과 Python의 자동 메모리 관리 시스템 개념 정리
---
* toc
{:toc .large-only}

# 가비지 컬렉션(Garbage Collection)

* 파이썬에서는 변수나 객체를 만들면, 그 값은 메모리의 힙(heap) 영역에 저장된다. 
* 하지만 프로그램을 계속 실행하면서 더 이상 사용하지 않는 객체들이 쌓이면 메모리 낭비가 발생한다. 
* 이때 가비지 컬렉터(GC)가 작동하여, 쓸모없는 객체를 찾아서 자동으로 메모리에서 제거한다.

## Python의 자동 메모리 관리 시스템

* `del`은 `heap`이 아닌 `stack`의 변수명만 지운다.
* `reference count`가 `0`인 것만 지운다.

![del&GC](https://github.com/user-attachments/assets/a92c17af-6557-49ba-a462-12b0e4765a08)

### 가비지 컬렉션 과정
1. 초기 상태
  * `name1`과 `name2`가 같은 객체 참조
  * **참조 카운트: `2`** (두 변수가 가리킴)

2. `del(name1)` 실행
  * **Stack**에서만 `name1` 변수 제거
  * ⚠️ **Heap의 객체는 여전히 존재**
  * **참조 카운트: `2` -> `1`** (name2가 아직 참조 중)

3. `del(name2)` 실행
  * **Stack**에서 `name2`도 제거
  * **Heap**의 객체는 **고아 객체** 상태
  * **참조 카운트: `1` -> `0`** (아무도 참조 안 함)

4. 가비지 컬렉션 실행
  * **참조 카운트가 `0`인 객체 탐지**
  * **자동으로 메모리 해제**
  * 메모리 공간 재활용 가능

## 🔑 핵심 포인트:

* **`del`은 변수만 삭제**: 객체를 직접 삭제하지 않음
* **참조 카운트(Reference Count)**: 객체를 가리키는 변수의 개수
* **가비지 컬렉션(GC)**: `ref_count`가 `0`이 되면 **자동으로 메모리 해제**
* **다른 참조가 있으면**: 객체는 계속 유지됨