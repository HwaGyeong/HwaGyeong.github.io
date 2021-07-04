---
layout: post
title : "[SQL] GROUP BY 한 결과로 UPDATE 하는 법"
subtitle: update SQL
categories:
  - troubleshooting
tags:
  - [SQL]
comments: true
---

## 기존 서비스 운영 중 DB를 업데이트 해야할 때
서비스 진행중에 새로운 서비스를 도입해서 기존의 사용자에게도 적용해야할 때,
조건을 걸고 일괄 수정이 필요할 때가 있다.
이번의 경우에는 id로 group by절을 사용하여 나온 결과의 수를 세고 그 결과대로 컬럼 두개를 업데이트 시켜야 했다

   
***
### 1. UPDATE가 필요한 조건의 대상자 찾기

> user의 id로 그룹화 하여 해당 유저가 작성한 글의 수를 센다

```bash
select count(*) as CNT, user_id from feedlist group by user_id having CNT>=7;
# Feedlist라는 테이블에서 user_id로 그룹화 하고 반환되는 튜플의 수를 세는데, 7개 이상일 때만 나오도록 한다. 
```
group에 대한 조건을 세기 위해서는 where가 아닌 HAVING을 사용해야 한다. 

***
### 2. 해당 조건으로 UPDATE를 실행해야 할 때 -> INNER JOIN을 사용한다 

이번에는 user테이블의 grade, point 컬럼을 업데이트 시켜야한다. 
1의 결과를 하나의 테이블이 구성되었다고 생각하고 inner join을 실행한다. 

```bash
update user as A 
inner join (select count(*) as CNT,user_id from feedlist group by user_id having CNT>=30) as B
on B.user_id=A.id
set A.grade=A.grade+1 , A.point=A.point+50
where B.user_id = A.id;
```
user 테이블을 A라고 하고, group by로 찾아낸 결과를 B 테이블인 것 처럼하고 join연산을 진행한 뒤 업데이트 해준다.