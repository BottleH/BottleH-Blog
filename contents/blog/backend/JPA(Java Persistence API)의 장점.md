---
title: JPA(Java Persistence API)의 장점
thumbnail: ./images/jpa/jpa.png
date: 2021-02-25 18:15:51
category: Back-End
tags: [Java, Spring, JPA]
draft: false
---

# JPA(Java Persistence API)의 장점
최근 프로젝트에서 시니어분들의 MyBatis(SQL Mapper 방식)와 JPA(ORM)의 사용에 대한 의견충돌이 있었다.(JPA 승!!) 이를 보고, 궁금하기도 하고 빨리 해보고 싶어서 JPA의 장점을 김영한 님의 강의와 공부를 통해서 알아보았다.



## 01. JPA란❓

**Java Persistence API**의 약어로 자바 진영의 ORM 표준 기술이다.

- ORM이란 Object-Relational Mapping(객체 관계 매핑)의 약어로 객체는 객체대로 설계하고 RDB는 RDB대로 설계한 뒤, ORM 프레임워크가 이를 중간에서 매핑시켜 주는 것이다!



## 02. JPA의 장점❗

1. SQL 중심으로의 개발에서 **객체 중심의 개발**
2. 생산성 증가
   - SQL Query를 짤 필요 없이 JPA기능을 이용하여 손쉽게 CRUD를 할 수 있다.
3. 유지보수성 증대
   - 기존: 필드 변경시 모든 SQL 수정
   - JPA: 필드만 추가하면 됨, SQL은 JPA가 처리
4. 패러다임의 불일치가 해결된다. 
   - 예를들어, RDB에는 상속이 존재하지 않는다! 이를 해결함.
5. 성능 증가
   - 1차 캐시와 동일성(identity) 보장
     - **같은 트랜잭션 안**에서는 **같은 엔티티를 반환**하여, 약간의 조회 성능 향상
     - DB Isolation Level이 **Read Commit**이어도 애플리케이션 Level에서 **Repeatable Read** 보장([Isolation Level 포스팅](https://bottleh.netlify.app/backend/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%20%EA%B2%A9%EB%A6%AC%EC%88%98%EC%A4%80(Transaction%EC%9D%98%20Isolation%20Level)/))
   - 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
     - 트랜잭션을 커밋할 때까지 INSERT SQL을 모음
     - JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송
     - UPDATE, DELETE로 인한 로우(ROW)락 시간 최소화
   - 지연 로딩(Lazy Loading)
     - 지연 로딩: 객체가 실제 사용될 때 로딩
     - 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회



> reference: [김영한님 강의](https://www.inflearn.com/course/ORM-JPA-Basic), [자바 ORM 표준 JPA 프로그래밍](http://www.yes24.com/Product/Goods/19040233)