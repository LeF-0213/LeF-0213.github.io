---
layout: post
related_posts:
    - /etc/git
title:  "Git & GitHub"
date:   2025-10-22
categories:
  - etc
  - git
description: >
  버전 관리 시스템 Git과 협업 플랫폼 GitHub에 대한 이해 및 명령어 정리
---
* toc
{:toc .large-only}

# Git이란?
**Git**은 분산 버전 관리 시스템(Version Control Systme)이다.

## 왜 Git이 필요할까?
### Git이 없을 경우
```
프로젝트_최종.zip
프로젝트_최종_진짜최종.zip
프로젝트_최종_진짜진짜최종_0105.zip
프로젝트_최종_이게진짜최종_0106_수정본.zip
```

### Git을 사용할 경우
```
git commit -m "기능 A 추가"
git commit -m "버그 수정"
git commit -m "기능 B 추가"
```
-> 모든 변경 이력이 자동으로 관리됨

## Git의 장점
* **변경 이력 추적** 📝: 누가, 언제, 무엇을, 왜 변경했는지 기록
* **되돌리기** 🔄: 이전 버전으로 쉽게 복구
* **브랜치** 🌿: 독립적인 작업 공간 생성
* **협업** 👥: 여러 사람이 동시에 작업 가능
* **백업** 💾: 분산 저장으로 데이터 손실 방지

# GitHub이란?
**GitHub**은 Git 저장소를 호스팅하는 웹 서비스이다.

## Git vs GitHub

| 구분 | Git | GitHub |
|:----:|:----------:|:--------:|
| 정체 | 버전 관리 **프로그램** | 웹 **서비스** |
| 설치 | 컴퓨터에 설치 필요 | 웹 브라우저로 접속 |
| 역할 | 로컬에서 버전 관리 | 원격 저장소 + 협업 |
| 비유 | 문서 작성 프로그램 | 클라우드 저장소 |

## GitHub의 기능
- 🌐 **원격 저장소**: 코드를 온라인에 저장
- 👥 **협업**: Pull Request, Issue, Code Review
- 📊 **프로젝트 관리**: Projects, Wiki, Actions
- 🌟 **오픈소스**: 전 세계 개발자와 공유
- 📈 **포트폴리오**: 개발 활동 기록

# Git 기본 명령어
### repository 생성하는 법
**GitHub**에서 내 **repository 카테고리**로 들어가 **new 버튼** 클릭 
![gitnewrepo1](https://github.com/user-attachments/assets/ee8dd1fd-9a2b-4407-b010-430df35f9c17)

**Repository name**에 원하는 이름 적고 **README**가 필요할 경우 키고 **Create repository 버튼** 클릭
![gitnewrepo1](https://github.com/user-attachments/assets/16cda2d4-ad4e-4466-b9ff-6f500ebc8f5b)

### 저장소 생성
**repository에 생성한 주소 혹은 clone할 주소**
![gitaddress](https://github.com/user-attachments/assets/adcf4a9f-f60c-46e0-b6c6-75efd1f3c1ca)

**그 후**
```bash
# 새 프로젝트 시작
cd 프로젝트명
git init

# 기존 프로젝트 복제
cd 프로젝트 생성할 곳
git init
 # repository에 생성한 주소 복사해서 붙여넣기
git clone https://github.com/username/repo.git
```
### 초기 설정

```bash
# 사용자 이름 설정
git config --global user.name "홍길동"

# 이메일 설정 (GitHub 이메일과 동일하게)
git config --global user.email "hong@example.com"

# 기본 브랜치 이름을 main으로 설정
git config --global init.defaultBranch main

# 설정 확인
git config --list
```


### 상태 확인

```bash
# 현재 상태 확인
git status

# 간략하게 보기
git status -s
```

### 변경사항 추가

```bash
# 특정 파일만 추가
git add file.txt

# 여러 파일 추가
git add file1.txt file2.txt

# 모든 변경사항 추가
git add .

# 특정 확장자만 추가
git add *.py
```
### 커밋

```bash
# 커밋 (메시지 입력)
git commit -m "Initial Commit"
```
### 히스토리 보기

```bash
# 커밋 로그 보기
git log

# 한 줄로 보기
git log --oneline

# 최근 5개만
git log -5

# 특정 파일의 히스토리
git log file.txt

# 그래프로 보기
git log --oneline --graph --all
```

# 원격 저장소 (Remote)
## GitHub 저장소 연결

```bash
# 원격 저장소 추가
git remote add origin https://github.com/username/repo.git # repository에 생성한 주소 복사해서 붙여넣기

# 원격 저장소 확인
git remote -v

# 원격 저장소 URL 변경
git remote set-url origin https://github.com/username/new-repo.git

# 원격 저장소 삭제
git remote remove origin
```
## Push(업로드)

```bash
# 처음 push할 때
git push -u origin main

# 이후부터는
git push origin main

# main이 아닌 특정 브랜치 push
git push origin feature/login
```
## Pull(다운로드)

```bash
# 특정 브랜치에서 가져오기
git pull origin main
```

```bash
# 사용자 이름_설정
git config --global user.name "홍길동"
# 이메일 설정
git config --global user.email "hong@example.com"
```

# 브랜치(Branch)
브랜치는 **독립적인 작업 공간**을 만드는 기능이다

## 왜 브랜치를 쓸까?

```
main (메인 브랜치)
  ↓
  ├─── feature/login (로그인 기능 개발)
  ├─── feature/payment (결제 기능 개발)
  └─── bugfix/header (헤더 버그 수정)
```
-> 각자 독립적으로 작업하다가 완료되면 main에 합침

## 브랜치 기본 명령어
```bash
# 브랜치 목록 보기
git branch

# 새 브랜치 생성
git branch feature/login

# 브랜치 이동
git checkout feature/login
# 또는
git switch feature/login

# 생성 + 이동 동시에
git checkout -b feature/login
# 또는
git switch -c feature/login

# 브랜치 삭제
git branch -d feature/login
```

## 충돌 (Conflict) 해결

```bash
# 병합 시도
git merge feature/login

# 충돌 발생!
Auto merging file.txt
CONFLICT (content): Merge conflict in file.txt
```
### 충돌 파일 내용

```python
<<<<<<< HEAD
print("main 브랜치의 코드")
=======
print("feature 브랜치의 코드")
>>>>>>> feature/login
```
### 해결방법
* 파일을 열어서 수동으로 수정
* `<<<<<<<`, `=======`, `>>>>>>>` 제거
* 원하는 코드만 남김
* `git add .` 또는 `git add file.txt`
* `git commit -m "[Conflict] 무슨 충돌 해결"`

## Conflict 발생 시 Tip
**내가 작업한 것을 모두 저장하여 내 브랜치에 올리기**

```bash
git add .
git commit -m "무슴 작업 완료"
git push origin feature/login
```
**내 개인적인 브랜치 작업 공간에 main 브랜치를 받아옴**

```bash
git pull origin main
# merge 발생
# conflict 해결 후
git push origin feature/login
git commit -m "[Conflict] 무슨 충돌 해결"
```

# 협업 워크플로우
## Fork & Pull Request 방식 (오픈 소스)

```bash
# GitHub에서 Fork
# 내 저장소를 클론
git clone https://github.com/내계정/repo.git
# 원본 저장소를 upstream으로 추가
git remote add upstream https://github.com/원본계정/repo.git
# 새 브랜치 생성
git checkout -b feature/new-feature
# 작업 후 커밋
git add .
git commit -m "새 기능 추가"
# 내 저장소에 push
git push origin feature/new-feature
# GitHub에서 Pull Request 생성
```
❗️ **GitHub에서 Pull Request(PR) 생성 시 확인 요청** (팀과 상의)

## Shared Repository 방식 (팀 프로젝트)

```bash
# 저장소 클론
git clone https://github.com/team/project.git
git remote add https://github.com/team/project.git
# 새 브랜치 생성
git checkout -b feature/my-feature
# 작업 후 커밋
git add .
git commit -m "기능 추가"
```

# 실전 시나리오
## 커밋을 되돌릴 때

```bash
# 되돌리고 싶은 커밋ID 위치로 (변경사항은 유지)
git reset --soft 커밋ID

# 되돌리고 싶은 커밋ID 위치로 (변경사항도 삭제되므로 주의)
git reset --hard 커밋ID
```
## 작업 중 급한 버그 수정
```bash
# 현재 작업 임시 저장
git stash

# 버그 수정 브랜치로 이동
git checkout main
git checkout -b hotfix/urgent-bug

# 버그 수정 후 커밋(hotfix/urgent-bug 브랜치)
git add .
git commit -m "긴급 버그 수정"

# 특정 브랜치에 병합
git checkout main
git merge hotfix/urgent-bug

# 원래 작업으로 복귀
git checkout feature/my-work
git stash pop
```

# Tip
## Git Flow
* `main` - 최종 배포 브렌치  
* `develop` - feature 브랜치에서 작업이 끝났을 경우 기능이 합쳐지는 브랜치  
* `hotfix` - 이슈 발생시 이슈 해결 브랜치  
* `feature` - 각 기능 작업 브랜치

## 커밋 메시지 컨벤션
예제: `Feat(#이슈번호): 커밋내용`
* `Struct` : 빌드 업무 수정, 패키지 매니저 수정
* `Feat` : 새로운 기능 추가
* `Fix` : 버그 수정
* `Docs` : 문서 수정
* `Style` : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
* `Refactor` : 코드 리펙토링
* `Test` : 테스트 코드, 리펙토링 테스트 코드 추가
* `Chore` : 빌드 업무 수정, 패키지 매니저 수정
* `Conflict`: 충돌 해결

## 이슈 타이틀 컨벤션
예제 `[Feat/Back]: 이슈 내용`
* `Struct` : 빌드 업무 수정, 패키지 매니저 수정
* `Feat` : 새로운 기능 추가
* `Fix` : 버그 수정
* `Docs` : 문서 수정
* `Style` : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
* `Refactor` : 코드 리펙토링
* `Test` : 테스트 코드, 리펙토링 테스트 코드 추가
* `Chore` : 빌드 업무 수정, 패키지 매니저 수정
* `Conflict`: 충돌 해결