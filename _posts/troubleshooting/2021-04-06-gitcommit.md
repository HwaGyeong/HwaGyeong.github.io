---
layout: post
title : "[GIT] GIT 커밋취소, gitIgnore"
subtitle: Git에 개인정보가 올라갔을 때
categories:
  - troubleshooting
tags:
  - [web, git]
comments: true
---

## GIT에 원치않는 개인정보가 올라갔을 때 

ignore파일에 파일명을 설정했는데도 실제로 그냥 올라가 버리는 경우가 있다.   
따라서 이럴땐 `커밋내용을 지우고 다시 gitignore 파일을 설정`해서 올려서 개인정보를 지워야한다. 아래와 같은 순서로 진행하면 다시 올릴 수 있다.
   
***
### 1. GIT 커밋 취소

> commit 내용 취소 

```bash
git log #커밋 이력을 확인한다
git reset HEAD^ #이전의 커밋내용 하나를 지운다
git push -f origin (branch 이름) #커밋내용을 지우고 다시 push
```
   

> commit 여러개를 지우고 싶은 경우    

```bash
git log #커밋 이력을 확인한다
git reset HEAD~3 #이전의 커밋내용 3개를 지운다
git push -f origin (branch 이름) #커밋내용을 지우고 다시 push
```
^대신 ~숫자를 입력하면 그 갯수만큼 커밋 이력이 지워진다
   

***
### 2. GIT ignore이 설정되지 않을 때 

ignore 파일이 적용되지 않은 이유는 cache때문이다.   
따라서 cache 내용을 지우고 다시 push 해주면 제대로 적용될 것이다.

```bash
git rm -r --cached . #cache 삭제
git add . #변경 내역 등록
git commit -m "커밋 메세지" #커밋 메세지 입력
git push #깃헙에 다시 올리기
```