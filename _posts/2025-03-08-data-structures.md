---
layout: post
title:  "자바의 자료구조에 대해서"
date:   2025-03-08
categories: jekyll update
---
* toc
{:toc .large-only}

# 자바의 자료구조(Java Data Structures)
자바의 자료구조는 크게 **기본 자료구조(배열)**과 **컬렉션 프레임워크(Collection FrameWork)**로 나눌 수 있다.

## 배열(Array)
가장 기본적인 자료구조로, 동일한 타입의 데이터를 연속된 메모리 공간에 저장한다.
```java
    int[] numbers = new int[5];
    String[] names = {"Alice", "Bob", "Charlie"};
```
* **특징**: 
  * 고정 크기
  * 빠른 접근 속도(`O(1)`)
  * 기본형/참조형 데이터 모두 저장 가능
* **단점**: 
  * 크기 변경 불가
  * 삽입/삭제 연산이 비효율적(`O(n)`)

## 컬렉션 프레임워크(Collection Framework)
컬렉션 프레임워크는 `java.util`패키지에 포함된 **자료구조 라이브러리**이다. 크게 **List, Set, Map, Queue**로 분류되어 다양한 데이터 구조와 알고리즘을 제공한다.
### List 인터페이스(순서가 있는 컬렉션)
#### ArrayList
동적 배열 구현체로, 내부적으로 배열을 사용하여 요소를 저장한다.
```java
    ArrayList<String> list = new ArrayList<>();
    list.add("Apple");  // 요소 추가, list = ["Apple"]
    list.get(0);    // 요소 접근, "Apple"
    list.remove(0); // 요소 삭제, list = []
```
* **특징**: 
  * 빠른 조회(`O(1)`)
  * 동적 크기 조정
  * 요소 중복 허용
  * 멀티스레드 환경에서 동기화 **X** ->  `Vector` 사용하면 동기화 지원
* **단점**: 
  * 삽입/삭제 시 요소 이동(`O(n)`)

#### LinkedList
이중 연결 리스트 구현체로, 각 노드가 데이터와 이전/다음 노드 참조를 가진다.
```java
    LinkedList<String> linkedList = new LinkedList<>();
    linkedList.add("Apple");    // linkedList = ["Apple"]
    linkedList.addFirst("Banana"); // linkedList = ["Banana", "Apple"]
    linkedList.removeLast();    // linkedList = ["Banana"]
```
* **특징**:
  * 빠른 삽입/삭제(`O(1)`)
  * 양방향 순회 가능
  * `Queue`와 `Deque` 인터페이스를 구현하여 큐처럼 사용 가능
* **단점**:
  * 요소 접근 시간이 느림(`O(n)`)
  * 메모리 사용량이 많음

#### Vector
ArrayList와 유사하지만 **동기화된(synchronized) 메서드**를 제공하고, **스레드 안전(Thread-safe)**하게 동작한다.
 (단, 일반적인 단일 스레드 환경에서는 `ArrayList`가 더 성능이 좋음)
```java
    Vector<String> vector = new Vector<>();
```
* **특징**:
  * 스레드 안전(Thread-safe)
  * ArrayList와 기능적으로 동일
* **단점**:
  * 동기화로 인한 성능 저하

#### Stack
Vector를 확장한 후입선출(**LIFO**, Last-In First-Out) 구조로, **마지막에 추가된 요소가 가장 먼저 제거되는 구조**이다.
```java
    Stack<String> stack = new Stack<>();
    stack.push("Apple");
    stack.pop();    // 맨 위(top) 요소 제거 및 반환
    stack.peek();   // 맨 위(top) 요소 조회
    stack.isEmpty(); // 스택이 비어있는지 확인    
```
* **특징**:
  * LIFO 구조
  * 스레드 안전(Thread-safe)
* **단점**:
  * Vector 기반으로 내부적으로 성능 제약 존재

### Set 인터페이스(중복을 허용하지 않는 컬렉션)
#### HashSet
해시 테이블을 사용하여 구현된 Set으로, 중복 요소를 허용하지 않는다.
```java
    HashSet<String> set = new HashSet<>();
    set.add("Apple");
    set.contains("Apple");  // 존재 여부 확인
```
* **특징**:
  * 빠른조회/삽입/삭제(`O(1)`)
  * 중복 요소 불허
  * 순서를 보장하지 않음
* **단점**:
  * 요소 순서가 유지되지 않음

#### LinkedHashSet
HashSet과 유사하지만 **요소의 삽입 순서를 유지**한다.
```java
    LinkedHashSet<String> linkedSet = new LinkedHashSet<>();
```
* **특징**:
  * 삽입 순서 유지
  * 중복 요소 불허
* **단점**:
  * HashSet보다 약간 느림(LinkedList 사용 때문)

#### TreeSet
정렬된 Set으로, 요소가 자연순서나 비교자(Comparator)에 따라 정렬된다.
```java
    TreeSet<Integer> treeSet = new TreeSet<>(Comparator.reverseOrder());
    treeSet.first();    // 첫 번쨰(가장 데이터값이 작은) 요소
    treeSet.last();     // 마지막(가장 데이터값이 큰) 요소
```
* **특징**:
  * 이진 검색 트리 기반(**Red-Black Tree)** 
  * 요소 자동 정렬(오름차순 기본)
  * 범위 검색 효율적(`O(log n)`)
* **단점**:
  * Null 저장 불가(Java 7부터)

### Map 인터페이스(키-값(Key-Value) 쌍을 저장하는 컬렉션)
#### HashMap
**해시 테이블(Hashtable) 기반**의 Map으로, 키-값 쌍을 저장한다.
```java
    HashMap<String, Integer> map = new HashMap<>();
    map.put("Apple", 10);
    map.get("Apple");   // 값 조회, 10
    map.remove("Apple");
```
* **특징**:
  * 빠른조회/삽입/삭제(`O(1)`)
  * 키(Key) 중복 불허
  * 값(Value) 중복 허용
  * 비동기 처리(동기화를 원하면 `Hashtable` 사용)
* **단점**:
  * 키 순서가 유지되지 않음(비동기 처리)

#### LinkedHashMap
삽입 순서나 접근 순서를 유지하는 HashMap이다.
```java
    LinkedHashMap<String, Integer> linkedMap = new LinkedHashMap<>();
```
* **특징**:
  * 삽입 순서 유지
  * 접근 순서 유지
* **단점**:
  * HashMap보다 약간 느림 

#### TreeMap
키(Key)가 정렬된 Map으로, 키가 자연 순서나 비교자에 따라 정렬된다.
```java
    TreeMap<String, Integer> treeMap = new TreeMap<>();
    treeMap.firstKey();     // 첫 번째(가장 작은) 키
    treeMap.lastEntry();    // 마지막(가장 큰) 키-값 쌍
```
* **특징**:
  * 이진 검색 트리 기반 (Red-Black Tree) 
  * Key 자동 정렬
  * 범위 검색 효율적(`O(log n)`)
* **단점**:

### Queue 인터페이스(선입선출(`LIFO`) 컬렉션)
#### LinkedList
`Queue 인터페이스`를 구현한 `이중 연결 리스트(Doubly Linked List)`이다.
```java
    Queue<String> queue = new LinkedList<>();
    queue.offer("Apple");   // 요소 추가
    queue.poll();           // 첫 요소 제거 및 반환
    queue.peek();           // 첫 요소 조회
```
* **특징**:
  * FIFO 구조
  * 빠른 삽입/삭제
* **단점**:

#### PriorityQueue
요소가 우선순위에 따라 정렬되는 큐(Queue)이다.
```java
    PriorityQueue<Integer> pq = new PriorityQueue<>();  // 기본 최소 힙
    pq.offer(3);
    pq.offer(1);
    pq.poll();      // 최솟값(1) 반환

    PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Collections.reverseOrder()); // 최대 힙
    maxPQ.offer(3);
    maxPQ.offer(1);
    maxPQ.poll();   // 최대값(3) 반환
```
* **특징**:
  * 내부적으로 **힙(Heap) 자료구조** 사용
  * `poll()`시 가장 낮은(혹은 가장 높은) 우선순위의 요소가 반환
  *  요소가 우선순위에 따라 자동 정렬
* **단점**:
  * 요소 삽입시마다 재정렬되므로 인덱스 접근 불가능
  * 정렬 유지로 인한 약간의 오버헤드

#### ArrayDeque
**양반향 큐(Deque)** 구현체로, 스택이나 큐로 사용 가능하며 **양쪽 끝에서 빠르게 요소를 추가하고 제거할 수 있다.**
```java
    ArrayDeque<String> deque = new ArrayDeque<>();
    deque.addFirst("Apple");    // 앞에 추가
    deque.addLast("Banana");    // 뒤에 추가
    deque.removeFirst();        // 앞 요소 제거
```
* **특징**:
  * 내부적으로 **배열을 사용하지만, 크기가 자동으로 조정됨**
  * `LinkedList`보다 빠름(연결 리스트의 포인터 저장 비용 없음)
  * `LinkedList`보다 더 좋은 메모리 지역성과 캐시 성능
  * 양쪽(`front`, `back`)에서 추가/삭제 빠름(`O(1)`)
  * `Deque` 인터페이스 구현체로, 스택(`LIFO`)과 큐(`FIFO`)처럼 모두 사용 가능
* **단점**:
  * `null`값 저장 불가능
  * 임의 접근 불가(고정된 인덱스가 아닌 원형 배열을 사용하여 큐처럼 동작하기 때문)

## 특수 자료구조
### BitSet
비트 벡터를 나타내는 클래스로, 비트 연산에 효율적이다.
```java
    BitSet bits = new BitSet(16);
    bits.set(0);    // 0번 비트를 true로 설정, true
    bits.clear(0);  // 0번 비트를 false로 해제, false
    bits.flip(0);   // 0번 비트 반전, true
    bits.get(0);    // 비트 확인, true
```
* **특징**:
  * 비트 단위로 데이터를 저장하여 메모리 절약
  * 자동 크기 조정
  * 비트 연산을 활용한 빠른 연산(AND, OR, XOR, NOT 등을 지원)
  * 정수 인덱스를 사용하여 개별 비트를 조작 가능
* **단점**:
  * 비트 크기 조절이 필요하면 성능 저하
  * 음수 인덱스 사용 불가
  * `null`값 저장 불가
  * 임의 접근이 제한적(`ArrayList<Boolean>`같은 리스트 형태가 나을 수 있음)
  * 직렬화(serialization)시 비효율적

#### EnumSet
Enum 타입의 요소를 효율적으로 저장하는 Set이다.
```java
    enum Dary { MONDAY, TUESDAY, WEDNESDAY }
    EnumSet<Day> weekdays = EnumSet.of(Day.MONDAY, Day.TUESDAY); // [MONDAY, TUESDAY]
```
* **특징**:
  * 오직 `Enum`요소만 저장할 수 있음
  * `Set` 인터페이스 구현
  * 비트 벡터(Bit Vector)기반 저장 방식(빠른 연산, `HashSet`보다 메모리 효율이 좋음)
  * **`Enum`선언 순서**에 따라 정렬
* **단점**:
  * `Null`값 저장 불가

#### WeakHashMap
키에 대한 **약한 참조(Weak Reference)**를 사용하는 **HashMap**이다. 즉, 키 객체가 더 이상 **강한 참조(Strong Reference)**를 가지지 않으면 **GC(Garbage Collector)**에 의해 자동으로 제거된다.
```java
    WeakHashMap<Key, Value> weakMap = new WeakHashMap<>();
```
* **특징**:
  * 키가 약한 참조를 가지므로 GC가 회수 가능
  * 키에 대한 강한 참조가 사라지면 해당 키-값 쌍이 자동 삭제
  * 캐싱, 메모리 관리 최적화(불필요한 객체 자동 정리)
* **단점**:
  * 동기화 기능이 없음

## 자료구조 선택 가이드
| 자료구조 | 사용 시나리오 |
|---------|--------------|
| **Array** | 크기가 고정된 데이터, 빠른 인덱스 액세스가 필요한 경우 |
| **ArrayList** | 동적 크기 조정이 필요하고 주로 요소 조회를 많이 하는 경우 |
| **LinkedList** | 요소 삽입/삭제가 빈번한 경우, 양방향 탐색이 필요한 경우 |
| **HashSet** | 중복 없이 요소를 저장하고 빠른 조회가 필요한 경우 |
| **TreeSet** | 정렬된 요소 집합이 필요한 경우, 범위 검색이 필요한 경우 |
| **HashMap** | 키-값 쌍으로 데이터를 저장하고 빠르게 조회해야 하는 경우 |
| **TreeMap** | 키-값 쌍을 키 기준으로 정렬해야 하는 경우 |
| **PriorityQueue** | 우선순위에 따라 요소를 처리해야 하는 경우 |
| **ArrayDeque** | 스택(LIFO)이나 큐(FIFO) 구현이 필요한 경우, Stack이나 LinkedList보다 더 효율적인 메모리 사용과 성능이 요구되는 경우 |