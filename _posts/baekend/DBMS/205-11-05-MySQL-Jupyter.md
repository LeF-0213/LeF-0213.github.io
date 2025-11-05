---
layout: post
related_posts:
  - /baekend/dbms
title: "MySQL 연동 + 객체지향 프로그래밍을 활용한 나만의 데이터베이스 프로그램 만들기(Jupyter Python 작업)"
date: 2025-11-05
categories:
  - baekend
  - dbms
  - python
description: >
---

- toc
  {:toc .large-only}

# 경험치를 통해 성장하는 루틴 관리 프로그램

## 다이어그램

![diagram](https://github.com/user-attachments/assets/a5dafbdd-b2b4-4ab4-901d-74fccc7dca67)

## Object

### User

![User](https://github.com/user-attachments/assets/d3a0202d-d897-4ee2-a2fc-61bfa460fbb0)

### Routine

![routine](https://github.com/user-attachments/assets/76af6d49-af08-4e0e-899a-468f073af508)

### RoutineGroup

![routinegroup](https://github.com/user-attachments/assets/fc8f4700-7c91-4fa8-b108-ae998b90cf7e)

### RoutineLogs

![routinelogs](https://github.com/user-attachments/assets/cc5c888b-7555-4346-821d-41fa0477d37b)

### DailyStats

![dailystats](https://github.com/user-attachments/assets/6b2e5a80-9842-4f7d-b686-4464df972a0c)

## DAO

### UsersDAO

![UsersDAO](https://github.com/user-attachments/assets/ceeda82f-e7ee-4395-8a6d-47c537aaf62a)

### GroupDAO

![groupdao](https://github.com/user-attachments/assets/b44373f5-096f-41dd-a0eb-7bd183392b7b)

### RoutineGroupItemsDAO

![groupitemdao](https://github.com/user-attachments/assets/00701978-a938-4e9e-86f2-228d1a17ab26)

### RoutineDAO

![routineDAO](https://github.com/user-attachments/assets/9f0fc42c-6b86-47fb-b603-470c184c66a3)

### RoutineLogDAO

![routinelogdao1](https://github.com/user-attachments/assets/b36f6de1-73b1-4812-9ebc-5fe1d158e170)

![routinelogdao2](https://github.com/user-attachments/assets/bc9be49e-2a03-4148-8f4e-55428d7ebc2d)

![routinelogdao3](https://github.com/user-attachments/assets/f3c6d695-96d9-404c-840a-b22a70f6c19a)

## Service

### UsersService

![UsersService]https://github.com/user-attachments/assets/158a8f5e-1965-4ca3-b046-fdbdeb3a6473)

### GroupsService + GroupRoutineItemService

![grouoproutineservice](https://github.com/user-attachments/assets/09d7536f-3b88-4939-8643-fb795c522eb7)

### RoutinesService

![routinesService](https://github.com/user-attachments/assets/2e34aab5-1295-4d2b-bd45-c6bddf62d558)

## Session

![session](https://github.com/user-attachments/assets/79398d5c-8624-400e-99a9-24c693887eaf)

## main

### Login & Register

![login](https://github.com/user-attachments/assets/28411580-4f46-4827-8e4a-2d3fad7f9012)

#### 출력 결과

![출력](https://github.com/user-attachments/assets/402d9dc8-640f-423c-871b-38e7def56875)

### 루틴 관리

![manageroutine](https://github.com/user-attachments/assets/cd7ee0f6-4f2f-4c63-ab47-1a52fbe622a7)

#### 출력 결과

##### 루틴 관리 보기

<img width="424" height="536" alt="Image" src="https://github.com/user-attachments/assets/75e8ec77-10aa-4e59-8aa3-c8aa5b854132" />

##### 루틴 그룹 생성하기

<img width="392" height="176" alt="Image" src="https://github.com/user-attachments/assets/de2493d0-533e-4c0f-9605-4f7f47675e2f" />

##### 그룹에서 루틴 추가하기

<img width="426" height="584" alt="Image" src="https://github.com/user-attachments/assets/52b287c8-8a1a-4ea0-a034-a4d70423cb4e" />

##### 그룹에서 루틴 비활성화하기

<img width="422" height="567" alt="Image" src="https://github.com/user-attachments/assets/65c5408c-1b0a-4530-8873-c98330d4d336" />

##### 그룹에서 루틴 삭제하기

<img width="444" height="694" alt="Image" src="https://github.com/user-attachments/assets/52dbdf02-7149-4aac-bb0b-5feea22cc7db" />

##### 루틴 그룹 삭제하기

<img width="439" height="524" alt="Image" src="https://github.com/user-attachments/assets/f3f2b913-c047-4dbe-924f-560a466dfb8a" />

아직 마무리가 덜 되어 주말에 몰아서 할 예정입니다... ㅠ(이틀은 빡센 것 같습니다 ㅠ)
