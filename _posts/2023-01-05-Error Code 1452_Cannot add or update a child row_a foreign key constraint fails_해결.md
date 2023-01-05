---
permalink: /2023-01-06-Error Code 1452_Cannot add or update a child row_a foreign key constraint fails ​해결 /
title: "[DB] [MYSQL] [Error Code 1452] 외래키 문제 해결"
date: 2023-01-06 01:00:00
toc: true
toc_sticky: true
toc_label: "DB"
categories:
- DB
tags:
- DB
- MYSQL
---
<br><br><br>

## ❗ 에러메세지

```sql
Error Code: 1452. Cannot add or update a child row: a foreign key constraint fails (`user`.`#sql-19e0_13e`, CONSTRAINT `vam_book_ibfk_1` FOREIGN KEY (`authorId`) REFERENCES `vam_author` (`authorId`))
```
<br><br><br>

## ❗ 상황
📌 **vam_book, vam_bcate, vam_author** 라는 테이블이 있다.

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/db1.png?raw=true">
</p>

📌 **vam_book**에는 기존에 넣어둔 튜플이 존재한다.

📌 이때 **vam_author** 와 **vam_bcate**에 각각 있는 **authorId**와 **cateCode**를 **vam_book** 테이블의 외래키로 설정하려고 한다.
```sql
alter table vam_book add foreign key (authorId) references vam_author(authorId);
alter table vam_book add foreign key (cateCode) references vam_bcate(cateCode);
```

📌 **authorId**와 **cateCode**를 외래키로 설정하자 에러가 발생했다.

<br><br><br>

## ❗ 원인
결론부터 보면 **참조 무결성**을 위배 했기때문에 발생한 에러이다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/db2.png?raw=true">
</p>

**참조 무결성이란 관계 데이터베이스 관계 모델에서 2개의 관련 있던 관계 변수 간의 일관성을 말한다.** 

예를들어 위의 그림을 보면 artist_id라는 컬럼을 두 테이블 모두 가지고 있는데 위의 테이블은 {1,2,3}의 값을 가지고 아래 테이블은 {3,4}의 값을 가지고 있는것을 볼 수 있다.

참조하려는 테이블의 artist_id가 서로 일치하지 않기 때문에 이는 참조 무결성을 위배한것이라고 볼 수 있다.

<br><br><br>

## ❗ 해결
**기존에 vam_book에 들어있던 데이터를 지움으로써 authorId 컬럼과 cateCode 컬럼의 무결성 문제를 해결했다.**

