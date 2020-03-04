---
layout: post
title: '[git] git 기본 익히기'
subtitle: 
categories: til
tags: til git
comments: true
date: 2020-03-04 18:30:17 +0900
lastmod: 2020-03-04 18:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

버전관리 하기 좋은 git 기본<br/>



## 기본 명령어
---


### 1. git 저장소(repository) 초기화 작업

```bash
$ git init
```

- 원하는 폴더를 저장소로 만들게 되면, `git bash`에서는 (`master`)라고 표기된다.
- 그리고 숨김폴더로 `.git/` 이 생성된다.




### 2. 커밋할 목록에 담기(Staging Area)

```bash
$ git add .
```

- 현재 작업 공간(Working Directory / Tree)의 변경사항을 커밋할 목록에 추가한다.(`add`)
- `.` 리눅스에서 현재 디렉토리(폴더)를 표기하는 방법으로, 현재 내 폴더에 있는 파일의 변경사항을 전부 추가한다.
- 단일 파일만 추가하려면 `git add "파일이름"` 
- 해당 폴더만 추가하려면 `git add "폴더이름/"` 




### 3. 커밋하기

```bash
$ git commit -m `_________`
```

- 커밋을 할때에는 해당하는 버전의 이력을 의미하는 메시지를 반드시 적어준다.
- 메시지는 지금 버전을 쉽게 이해할 수 있도록 작성한다.
- 커밋은 현재 코드의 상태를 스냅샷 찍는 것이다.




### 4. 로그 확인하기

```bash
$ git log

commit 04c1f9122a59cc545264a50444e7a209b63e860b (HEAD -> master)
Author: gangpro <gangpro@email.com>
Date:   Fri May 24 11:09:31 2019 +0900

      modify commit

```

- 현재까지 커밋된 모든 이력을 확인 할 수 있다.




### 5. git 상태 확인하기

```bash
$ git status
```

- CLI (Command Line Interface) 현재 상태를 알기 위해 반드시 명령어를 통해 확인한다.
- 커밋할 목록에 담겨 있는지 혹은 untracked인지, 커밋할 내역이 있는지 등등 다양한 정보를 제공한다.



## 원격 저장소 활용하기

---

### 1. 원격저장소(remote repository) 등록하기

```bash
$ git remote add origin ___경로___
```

- 원격 저장소(`remote`)를 등록(`add`)한다. origin 이름으로 경로를!
- 최초에 한번만 등록하면 된다.
- 아래의 명령어로 현재 등록된 원격 저장소를 확인할 수 있다.



```bash
$ git remote --v
origin  https://github.com/gangpro/TIL.git (fetch)
origin  https://github.com/gangpro/TIL.git (push)
```



### 2. 원격 저장소에 올리기(`push`)

```bash
$ git push origin master
```

- git! 올려줘!(`push`) `origin` 이라는 이름의 원격저장소에 `master` 로!




### 3. 원격 저장소로부터 가져오기(`pull`)

```bash
$ git pull origin master
```


### 4. 원격 저장소에 복제(`clone`)하기

```bash
$ git clone ___경로___
```

- 다운 받기를 원하는 폴더에서 `git bash` 를 열고 위의 명령어를 입력한다.
- 경로는 `github`  에서 우측에 있는 초록색 버튼을 누르면 나타난다.






## 참고
공식 사이트 : [https://git-scm.com/book/ko/v2](https://git-scm.com/book/ko/v2)<br />

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>