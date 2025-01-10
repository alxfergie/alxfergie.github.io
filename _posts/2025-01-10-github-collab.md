---
date: 2025-01-10-github-collab
layout: post
title: Github를 통한 플랫폼 개발 방법 안내서
subtitle: '처음 Github를 통해 버전관리를 하고 싶은 모든 분들을 위한 안내서'
description: >-
  현업에서 개발자들은 깃허브를 이용해 여러 버전관리를 진행해왔다. 우리 람다에서는 각 트랙의 성과공유를 위한 블로그를 이 깃허브 콜라보레이터 방법을 이용해서 진행해보려고 한다.
image: 2025-01-10-github-collab/cover.png
optimized_image: 2025-01-10-github-collab/cover.png
  
category: github
tags:
  - collaborator
  - git
  - github
author: dongckim
paginate: false
---

## 왜 Github Collaborator를 알아야하나요?

깃허브는 개발자들의 숙명과 같은 존재이다. 아마 깃허브가 존재하기 때문에, 개발자들끼리의 개발과 협력이 존재했다고 해도 과언이 아니라고 본다.

여기에는 기본적으로 5가지 이유가 있다.
### Why 비개발자 should know Github Collab-System?
1. 기술 용어와 문화 익히기
- GitHub 사용을 통해 개발자들이 사용하는 용어와 협업 방식을 자연스럽게 익히게 되어, 기술적 대화에서 더 효과적으로 참여를 기대해본다.

2. 문서와 잔여 기록 보존
- 기획, 설계, 피드백, 리뷰 등의 활동도 GitHub에 기록하여 팀의 공동 자산으로 보관이 가능하다. 이는 나중에 프로젝트를 복기하거나 프레젠테이션 자료로 활용할 때 유용하다.

3. 기술 프로젝트 참여 경험
- GitHub Collaborator로 참여하면 기술 프로젝트의 협업 프로세스를 경험할 수 있어 비전공자도 기술 중심 환경에서의 경험을 쌓을 수 있다. 이는 커리어 확장에도 유용하다고 믿는다.

4. 버전 관리에 대한 이해
- 프로젝트가 어떻게 발전하고 있는지, 어떤 변경 사항이 있었는지를 추적할 수 있다. 비전공자도 문서 작성, 아이디어 정리 등 비기술적 기여를 기록하고 관리할 수 있는 확장성을 떠올려 볼 수 있을 것이다.

5. 협업 기술 강화
- GitHub는 기술 중심의 플랫폼이지만, Collaborator 기능은 비기술적인 업무(기획, 디자인, 문서화)와 기술적인 업무를 연결하는 다리 역할도 가능하다. 이를 통해 기술 팀과 원활한 소통을 기대해본다.



우리 동아리 LAMBDA에서는 개발자가 아닌 학생이라도, `트랙 리더`나 `임원진`이라면 Github 콜라보레이트 시스템을 이해하도록 요구하고 있다.


## 그래서 어떻게 하는데?

기본적인 프로세스는 이렇다.
1. Fork
2. clone, remote설정
3. branch 생성
4. 수정 작업 후 add, commit, push
5. Pull Request 생성
6. 코드리뷰, Merge Pull Reqest
7. Merge 이후 branch 삭제 및 동기화

자세히 알아보자.

### 1. Fork
타겟 프로젝트의 저장소를 자신의 저장소로 Fork 한다.
![]({{site.url}}/assets/img/2025-01-10-github-collab/githubmain.png)

나는 내 계정이라 그렇지만, blur처리된 `Fork` 버튼이 보일 것이다. 내 원격 서버에 접속하여, 모든 소스코드를 fork 떠오도록 해보자.

Fork가 완료되면, 자신의 계정에 새로운 저장소가 생길 것이다.

### 2. Clone, Remote 설정

![]({{site.url}}/assets/img/2025-01-10-github-collab/githubclone.png)
fork로 생성한 본인 계정의 저장소에서 clone or download 버튼을 누르고 표시되는 url을 복사한다. (중요 - 브라우저 url을 그냥 복사하면 안 된다)

- Mac기준 터미널을 켠다.
- 자신의 로컬에서 작업을 하기 위해 Fork한 저장소를 로컬에 clone을 한다.

```bash
$ git clone https://어쩌구 저쩌구
```

- 로컬 저장소에 원격 저장소를 추가한다. 위 작업과 동일하게 github 저장소에서 clone or download 메뉴를 통해서 확인한 url을 사용한다.

```bash
# 원본 프로젝트 저장소를 원격 저장소로 추가
$ git remote add https://github.com/계정 정보 어쩌구 저쩌구

# 원격 저장소 설정 현황 확인방법
$ git remote -v
```

### branch 생성

자신의 로컬 컴퓨터에서 코드를 추가하는 작업은 branch를 만들어서 진행한다.

> 개발을 하다 보면 코드를 여러 개로 복사해야 하는 일이 자주 생긴다. 코드를 통째로 복사하고 나서 원래 코드와는 상관없이 독립적으로 개발을 진행할 수 있는데, 이렇게 독립적으로 개발하는 것이 브랜치다. 

```bash
# 날짜이름(컨벤션) 이라는 이름의 branch를 생성한다.
$ git checkout -b 20250110
Switched to a new branch '20250110'

# 이제 2개의 브랜치가 존재한다.
$ git branch
* 20250110
  master

# 이후 :wq로 창을 나오면 된다.
```

### 수정 작업 후 add, commit, push

- 자신이 사용하는 코드 편집 툴을 활용하여 수정 작업을 진행한다.
- 작업이 완료되면, add, commit, push를 통해서 자신의 github repository (origin)에 수정사항을 반영한다.
- 주의사항 push 진행시에 branch 이름을 명시해주어야 한다.

```bash
# develop 브랜치의 수정 내역을 origin 으로 푸시한다.
$ git push origin 20250110
```

### Pull Request 생성
push 완료 후 본인 계정의 github 저장소에 들어오면 Compare & pull reqeust 버튼이 활성화 되어 있다.
- 해당 버튼을 선택하여 메시지를 작성하고 PR을 생성한다.
- 만약 생성이 안될 수도 있는데, 그땐 수동으로 만들어도 괜찮다.(관리자 승인 후)

![]({{site.url}}/assets/img/2025-01-10-github-collab/comparepull.png)

- 이후 Open a Pull Request 창이 뜰텐데, 여기서 오른쪽에 보이는 Assignee를 본인 관리자를 지정한 후, 안에 승인 받을 내용을 작성한 이후에 Create a pull request 버튼을 누르면 되겠다.

### 코드리뷰, Merge Pull Request
- PR을 받은 Assignee 관리자는 코드 변경내역을 확인하고 Merge 여부를 결정한다.

Master 브랜치로 Compare&Merge는 최종검토 후 배포 여부 결정한 이후 임원진만 하도록 합시다.

### Merge가 완료된 이후.
- Merge가 완료된 이후에는 로컬 코드와 원본 저장소의 코드를 동기화 해야한다.

```bash
# 본인 브랜치에서 master branch로 나가기
$ git checkout master
```

- Master 브랜치로 넘어왔다면, git pull을 하라는 명령어가 보일 것이다.

![]({{site.url}}/assets/img/2025-01-10-github-collab/terminal.png)

> 원격 서버의 Master는 당신이 넣은 코드와 merge가 되면서 모든 코드가 최신화 되었다. 하지만 당신의 로컬 Master에는 아직 Merge가 되기 전이기 때문에, 최신화가 필요할 것이다. 그래서 원격서버로부터 `pull` 당겨온다는 뜻이다.

```bash
# 코드 동기화
$ git pull 
# 브랜치 삭제
$ git branch -d 20250110
```

같은 방식으로 계속 진행하면 될 것 같다.

![]({{site.url}}/assets/img/2025-01-10-github-collab/gitlog.png)

위 사진 처럼 하나의 분기점에서 당신의 브랜치가 생성되어 버전을 만들고, 이 버전이 다시 원본에 합쳐지면서 하나의 큰 줄기가 만들어진다. 

이 모든 과정을 `버전관리` 라고 부른다.


이 설명서만 따라한다면, 당신은 벌써 기술 프로젝트에 참여하고 있는 기획자라는 점에서 시장가치, 몸값이 이미 2배는 뛰었다. (농담이다 그냥 넘어가라)