---
layout: post
related_posts:
    - /baekend/java
    - /baekend/java/2025-03-14-Access-Modifiers
    - /baekend/java/2025-03-29-Outer-Class
title:  "자바 내부 클래스"
date:   2025-03-22
categories:
  - baekend
  - java
description: >
  Java의 내부 클래스의 종류와 특징
---
* toc
{:toc .large-only}

# 내부 클래스 (Inner Class)
클래스 내부에 선언된 또다른 클래스
```java
    class OuterClass {
        private int outerField = 10;

        class InnerClass {
            void display() {
                System.out.println("외부 클래스의 필드 값: " + outerField);
            }
        }
    }
```
* 다른 클래스 내부에 정의된 클래스
* 외부 클래스의 멤버에 쉽게 접근 할 수 있으며, 논리적으로 그룹화된 클래스를 만들때 유용
* 코드의 **가독성, 캡슐화, 유지보수성**을 높이는 데 도움

## 멤버 중첩 클래스(Member Nested Classes)
### 인스턴스 멤버 클래스(Instance Member Class)
외부 클래스의 객체를 선언해야만 내부 클래스(인스턴스 멤버 클래스)를 사용할 수 있다.
```java
    class OuterClass {
        private int outerField = 10;

        // 인스턴스 멤버 클래스
        class InstanceMemberClass {
            private int innerField = 20;

            void display() {
                // 외부 클래스의 private 멤버에 접근 가능
                System.out.println("외부 필드: " + outerField);
                // this로 내부/외부 클래스 구분 가능
                System.out.println("내부 필드: " + this.innerField);
                // 만약 내부 클래스에 outerField라는 같은 이름의 변수가 존재한다면
                System.out.println("외부 this 참조: " + OuterClass.this.outerField);
            }
        }

        void createInner() {
            InstanceMemberClass inner = new InstanceMemberClass();
            inner.display();
        }
    }
```
**특징:**
* 외부 클래스의 인스턴스가 있어야 생성 가능
* 외부 클래스의 모든 멤버(private 포함)에 접근 가능
* `static` 멤버를 가질 수 없음
* 외부에서 생성 시: `OuterClass.IntanceInnerClass inner = outerInstance.new InstanceInnerClass();`
* 내부 클래스 내에서 `OuterClass.this`를 통해 외부 클래스 인스턴스 참조 가능

### 정적 멤버 클래스(Static Member Class)
외부 클래스에 바로 접근할 수 있는 내부 클래스(정적 멤버 클래스)
```java
    private static int staticOuterField = 30;
    private int instanceOuterField = 40;

    // 정적 내부 클래스
    static class StaticNestedClass {
        private int innerField = 50;
        private static int staticInnerField =60;

        void display() {
            // 외부 클래스의 static 멤버만 접근 가능
            System.out.println("외부 정적 필드: " + staticOuterField);

            // 외부 클래스의 인스턴스 멤버에는 접근 불가
            // System.out.println(instanceOuterField);  // 컴파일 에러
        }

        static void staticDisaplay() {
            System.out.println("정적 메소드 내에서 외부 정적 필드: " + staticOuterField);
            System.out.println("정적 메소드 내에서 내부 인스턴스 필드: " + this.innerField);
            System.out.println("정적 메소드 내에서 내부 정적 필드: " + staticInnerField);
        }
    }
```
**특징:**
* 외부 클래스의 인스턴스 없이 생성 가능
* 외부 클래스의 `static`멤버만 접근 가능
* `static`멤버를 가질 수 있음
* 엄밀히 말하면 '내부 클래스'가 아닌 '중첩 클래스'
* 외부에서 생성 시: `OuterClass.StaticNestedClass nestedInstance = new OuterClass.StaticNestedClass();`

## 지역 중첩 클래스(Local Nested Class)
메소드나 특정 블록 내에서 정의되는 클래스(메소드가 실행될 때만 지역 내부 클래스 사용)
```java
    class OuterClass {
        private int outerField = 70;

        void method(final int param) {
            final int localVar = 80;    // 자바 8부터는 effetively final 이면 final 키워드 생략 가능
            int effectivelyFinalVar = 90;   // 값이 변경되지 않으면 effectively final

            // 지역 내부 클래스
            class LocalInnerClass {
                private int innerField = 100;

                void display() {
                    // 외부 클래스의 멤버에 접근 가능
                    System.out.println("외부 필드: " + outerField);

                    // final 또는 effectively final 지역 변수와 매개변수에 접근 가능
                    System.out.println("메소드 final 매개변수: " + param);
                    System.out.println("final 지역 변수: " + localVar);
                    System.out.println("effectively final 변수: " + effectivelyFinalVar);
                }
            }

            // 지역 내부 클래스는 해당 메소드 내에서만 인스턴스화 가능
            LocalInnerClass local = new LocalInnerClass();
            local.display();

            // effectivelyFinalVar = 100;   // 값을 변경하면 지역 내부 클래스에서 사용 불가, 컴파일 에러 발생
        }
    }
```
**특징:**
* 선언된 블록 내에서만 사용 가능
* 외부 클래스의 모든 멤버와 final(또는 effectively final)지역 변수/매개변수에 접근 가능
* 접근 제한자(public, private 등) 사용 불가
* `static`멤버를 가질 수 없음
* 지역 변수 캡처 시 해당 변수는 final 또는 effectively final이어야 함

## 익명 내부 클래스(Anonymous Inner Class)
이름이 없는 지역 클래스로, 선언과 동시에 객체를 생성한다.
```java
    interface Clickable {
        void onClick();
    }
    
    class Button {
        private String label;

        Button(String label) {
            this.label = label;
        }

        String getLabel() {
            return label;
        }

        void setOnClickListener(Clickable listener) {
            // 리스너 등록 로직
        }
    }

    class Main {
        private int count = 0;

        void setupButton() {
            final String message = "클림됨";
            Button button = new Button("확인");

            // 익명 내부 클래스
            button.setOnClickListener(new Clickable() {
                @Override
                public void onClick() {
                    count++;    // 외부 클래스의 멤버 변수 접근
                    System.out.println(message + ": " + button.getLabel() + ", 카운트: " + count); 
                }
            });

            // 자바 8부터는 함수형 인터페이스의 경우 람다 표현식으로 대체 가능
            // button.setOnClickListener(() -> {
            //      count++;
            //      System.out.println(message + ": " + button.getLabel() + ",카운트: " + count);
            // });
        }

        // 클래스 확장을 위한 익명 내부 클래스
        Button createCustomButton() {
            return new Button("커스텀") {
                @Override
                public String getLabel() {
                    return "커스텀: " + super.getLabel();
                }

                // 새로운 메소드 추가 가능(하지만 외부에서는 타입 캐스팅 없이 접근 불가)
                public void customMethod() {
                    System.out.println("커스텀 메소드");
                }
            }
        }
    }
```
**특징**:
* 이름이 없으며 선언과 동시에 객체 생성
* 단 한 번만 사용되는 일회용 클래스
* 하나의 클래스나 인터페이스만 상속/구현 가능
* 생성자를 가질 수 없음
* 지역 내부 클래스와 동일한 제약 사항 적용(final 또는 effectively final 변수에만 접근 가능)
* 자바 8부터는 함수형 인터페이스의 경우 람다 표현식으로 대체 가능
* 메소드를 추가할 수 있지만, 선언된 타입의 메소드만 외부에서 접근 가능

## 내부 클래스 사용의 장점
* **캡슐화 강화**: 내부 클래스를 외부 클래스의 멤버처럼 선언하여 외부에서 접근을 제한
* **코드의 논리적 그룹화**: 논리적으로 연관된 클래스를 함께 유지
* **외부 클래스 멤버에 쉬운 접근**: 내부 클래스에서 외부 클래스의 private 멤버에도 접근 가능 
* **이벤트 처리 및 콜백 구현에 유용**: 특히 익명 클래스는 이벤트 핸들러 구현에 자주 사용
* **가독성과 유지보수성 향상**: 관련 코드를 함께 배치하여 가독성 향상

## 내부 클래스 사용시 주의사항
* **메모리 사용**: 인스턴스 내부 클래스는 외부 클래스에 대한 참조를 유지하므로 메모리 누수 가능성
* **복잡성 증가**: 과도한 내부 클래스 사용은 코드 복잡성 증가
* **직렬화 주의**: 내부 클래스의 직렬화는 특별한 처리가 필요할 수 있음
* **다이아몬드 연산자 제한**: 익명 내부 클래스에서는 다이아몬드 연산자(`<>`) 사용 제한