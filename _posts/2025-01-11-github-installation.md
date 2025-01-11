---
date: 2025-01-11-github-install
layout: post
title: Git 다운로드와 로컬 환경설정 정리
subtitle: '처음 Git을 설치하는 부원들을 위한 안내서'
description: >-
  성과공유용 Lambda-flix를 엑세스하기 위해서는 깃허브.
image: 2025-01-11-github-install/gitinstall.png
optimized_image: 2025-01-11-github-install/gitinstall.png
category: github
tags:
  - git
author: dongckim
paginate: false
---

## Git 설치 & 환경설정

1. Git 설치하기: [여기로 접속하기](https://git-scm.com/)

2. 설치 완료 후 Git Bash 열기
![]({{site.url}}/assets/img/2025-01-11-github-install/gitbash.png)

3. git bash 에서 환경설정 하기

- Step 1: 유저이름 설정

```bash
$ git config --global user.name "your_name"
```

- Step 2: 유저 이메일 설정하기

Github가입시 사용한 이메일을 쓰면 됨.

```bash
$ git config --global user.email "your_email"
```

## Github에 처음 코드 업로드하기

1. 초기화
```bash
$ git init
```

2. 추가할 파일 더하기
```bash
# add와 . 사이에 띄어쓰기 있음
$ git add .
```

3. 상태 확인하기
```bash
$ git status
```

4. 히스토리 만들기
```bash
$ git commit -m "커밋 메세지 남기기"
```

5. Github repository랑 내 로컬 프로젝트랑 연결
```bash
$ git remote add origin https://github.com/bitnaGithub/firstproject.git
```
- 아마 fork를 떠온 프로젝트를 한다면 자동으로 원격서버 연결이 되어있을 것이다.

6. Github 원격서버에 올리기
```bash
$ git push origin master
```

## Git Convention

1. Branch 명명법
```bash
$ git checkout -b 20250110 
```
- 포스팅 업로드 날짜로 브랜치를 생성해서 작성한다.
- 해당 포스팅과 관련된 내용 수정 및 작업은 타 브랜치로 **절대로** 넘어가지 않도록 한다.

2. Git Commit 컨벤션
```bash
$ git commit -m "[포스팅날짜(=브랜치 이름)] *** 포스팅 완료"
```
- 아무래도 포스팅이 주로 이루어질 것으로 예상되기 때문에, 포스팅과 관련된 모든 커밋 히스토리는 "[20250110] github 안내서 포스팅 완료" 이 형식으로 작성한다. 
- 엔지니어 포지션의 경우, 홈페이지 유지 보수하면서 코드 수정을 하게 되는 경우, `fix`, `style`, `refactor` 등등의 동사를 앞에 붙인 뒤 수정내용을 작성하는 식으로 커밋 메세지 컨벤션을 통일한다.