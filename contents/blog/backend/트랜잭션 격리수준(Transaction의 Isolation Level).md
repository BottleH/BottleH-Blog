---
title: 트랜잭션 격리수준(Transaction의 Isolation Level)
thumbnail: ./images/db/db.png
date: 2021-03-03 13:30:51
category: Back-End
tags: [DB]
draft: false
---

# Transaction의 Isolation Level



## 01.  Transaction Isolation Level(트랜잭션 격리수준) 이란❓

동시에 여러 트랜잭션이 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있는 수준

- DB 엔진은 ACID 원칙을 희생해 동시성을 얻을 수 있는 방법을 제공한다. 바로 **Transaction의 Isolation Level**이다.
- [ACID 포스팅](https://bottleh.netlify.app/backend/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98(Transaction)%EA%B3%BC%20%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98%20%ED%8A%B9%EC%84%B1(ACID)/)



## 02. Transaction Isolation Level 종류

Transaction Isolation Level은 상위레벨로 갈수록 **격리수준이 높아지고 동시성은 떨어진다.**



### Level 0. Read UnCommitted

- 다른 트랜잭션의 변경 내용이 commit/rollback 되지 않아도 읽을 수 있다.
- 정합성이 맞지않을 수 있기 때문에 권장하지 않는다.
- `Dirty read`현상 발생
  - 트랜잭션이 반영되지 않았는데 다른 트랜잭션에서 변경된 값을 읽게되는것

### Level 1. Read Committed

- 다른 트랜잭션이 commit한 내용만 읽을 수 있도록 하는 격리수준
- 대부분의 RDB가 사용한다.
- `NON_REPEATABLE_READ` 현상 발생
  - 배타적 잠금(쓰기 잠금)이 걸리기 전에 읽은 값이 직후에 변경된다면 다시 조회했을 때 처음과 다른 값이 나온다

### Level 2. Repeatable Read

- 트랜잭션마다 트랜잭션ID를 부여하여 자신의 트랜잭션ID보다 낮은ID가 commit한 값만 읽어오도록 한다.
- `Phantom read` 현상 발생
  - 만약 자신의 트랜잭션ID보다 낮은 ID를 가진 트랜잭션을 수행하게되면 레코드값이 보였다 안보였다 하는 문제
  - 이를 해결하기위해선 쓰기 잠금을 걸어놔야 한다.
- MySQL의 InnoDB 엔진의 레벨임!!(타 RDB와 다르다.)

### Level 3. Serializable

- 가장 엄격한 격리수준이며, 한 세션이 잡고 있는 동안 다른 세션의 트랜잭션이 모두 접근할 수 없도록 하는 격리 수준
- 동시성이 가장 낮다.



| Isolation level | Dirty Read | Non-Repeatable Read | Phantom Read |
| --------------- | ---------- | ------------------- | ------------ |
| Read Uncommited | O          | O                   | O            |
| Read Commited   | X          | O                   | O            |
| Repeatable Read | X          | X                   | O            |
| Serializable    | X          | X                   | X            |



> reference: [Bryce님 블로그](https://bryceyangs.github.io/study/2021/04/24/Database-DB-Lock/)
