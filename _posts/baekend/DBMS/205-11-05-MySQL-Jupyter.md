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
* toc
{:toc .large-only}

# 경험치를 통해 성장하는 루틴 관리 프로그램

## 다이어그램
![diagram](https://github.com/user-attachments/assets/a5dafbdd-b2b4-4ab4-901d-74fccc7dca67)

## Object

### User
<img width="729" height="173" alt="User" src="https://github.com/user-attachments/assets/a75c440e-671f-4aee-9228-ae7852dd8fff" />

### Routine
<img width="730" height="138" alt="Routine" src="https://github.com/user-attachments/assets/533f8d57-1bbc-40f3-8734-e99bbbb02af8" />

### RoutineGroup
<img width="730" height="103" alt="RoutinesGroup" src="https://github.com/user-attachments/assets/5f0cbc3f-58ca-451a-aead-e9da4abcb63f" />

### RoutineLogs
<img width="730" height="137" alt="RoutineLogs" src="https://github.com/user-attachments/assets/e08452d5-54b0-4890-9310-20dd09db2b7f" />

### DailyStats
<img width="729" height="114" alt="DailyStats" src="https://github.com/user-attachments/assets/bffce338-c4eb-44fe-8da3-8ca2cefe2b9a" />

## DAO

### UsersDAO
<img width="730" height="624" alt="UsersDAO" src="https://github.com/user-attachments/assets/49e39f1a-ad0d-4d04-a871-f0c5833a3ec8" />

### GroupDAO
<img width="730" height="559" alt="GroupDAO" src="https://github.com/user-attachments/assets/6a1466c0-0b3e-4268-bac2-be0dab8f6e56" />

### RoutineGroupItemsDAO
<img width="729" height="356" alt="GroupItemDAO" src="https://github.com/user-attachments/assets/0c23c89a-1574-42bc-bfbf-72f6d35fde27" />

### RoutineDAO
<img width="730" height="531" alt="RoutineDAO" src="https://github.com/user-attachments/assets/f6d2683e-acea-43cb-969a-0b1bbf2c232a" />

### RoutineLogDAO

<img width="729" height="586" alt="RoutineLogDAO1" src="https://github.com/user-attachments/assets/ba00eca0-b736-4398-9cb5-1e53063d339c" />
<img width="730" height="449" alt="RoutineLogDAO2" src="https://github.com/user-attachments/assets/3f10f81e-7bef-4538-9acf-e26b4eb6f823" />
<img width="729" height="432" alt="RoutineLogDAO3" src="https://github.com/user-attachments/assets/06824cd6-bbf3-4d89-9a6b-c61baf7f98a6" />

## Service

### UsersService
<img width="731" height="357" alt="UsersService" src="https://github.com/user-attachments/assets/4f6494bb-cd5b-418d-a6da-a446540d2569" />

### GroupsService + GroupRoutineItemService
<img width="729" height="331" alt="GroupRoutineService" src="https://github.com/user-attachments/assets/c0869bd3-bcd1-41aa-911d-c4a7c292ae53" />

### RoutinesService
<img width="730" height="422" alt="RoutineService" src="https://github.com/user-attachments/assets/e67b2b67-9a6c-47b0-b465-e700abb2915a" />

### RoutineLogService
<img width="730" height="231" alt="RoutineLogService
" src="https://github.com/user-attachments/assets/ee7cae5f-b4ca-481f-b3cc-bbdd0d429813" />

## Session
<img width="729" height="177" alt="Session" src="https://github.com/user-attachments/assets/bd56e51d-72fb-48f0-a4ae-35b2f8274234" />

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

### 오늘의 루틴
<img width="974" height="465" alt="Image" src="https://github.com/user-attachments/assets/bffa67c2-500e-47f9-9ae7-dcb1a7a342c7" />

#### 오늘의 루틴 보기
<img width="978" height="250" alt="Image" src="https://github.com/user-attachments/assets/65f187bb-a4ff-47fd-ab97-452963b9c674" />

#### 오늘의 진행도 보기
<img width="967" height="145" alt="Image" src="https://github.com/user-attachments/assets/15514fad-354b-48f5-8e48-d41dc92dac6f" />

#### 루틴 체크하기
<img width="947" height="384" alt="Image" src="https://github.com/user-attachments/assets/439e46fb-c6f7-442a-82f4-e04a9ea7acbf" />

#### 새 루틴 추가하기
<img width="950" height="336" alt="Image" src="https://github.com/user-attachments/assets/47e7f4d9-c994-432d-947b-182be38812d6" />

#### 루틴 수정하기
<img width="960" height="354" alt="Image" src="https://github.com/user-attachments/assets/aae33a3f-cf00-4fcd-b8fc-545c876cc531" />
