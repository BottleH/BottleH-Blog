---
title: Spring Data JPA의 save vs saveAndFlush
thumbnail: ./images/jpa/jpa.png
date: 2021-07-02 16:53:51
category: Back-End
tags: [Java, Spring, JPA]
draft: false
---

# Spring Data JPA의 save 와 saveAndFlush의 차이점
최근에 `JpaRepository`의 `save`와 `saveAndFlush`의 차이점이 무엇이냐는 질문을 받았고, 공부한 것을 정리해 보았다!



## 01. Flush❓

플러시(Flush)란 영속성 컨텍스트의 변경 내용을 DB에 반영하는 것이다.



## 02. save vs saveAndFlush

### save

- `CrudRepository`에 포함된다.
- 명시적으로 플러시 및 커밋 메서드를 호출하지 않는 한 데이터베이스에 직접 데이터를 플러시하지 않는다.
- bulk insert 제공



### saveAndFlush

- `JPARepository`에 포함된다.
- 데이터를 데이터베이스에 직접 플러시합니다.
- bulk insert 미제공

|         Key         |                             Save                             |                         SaveAndFlush                         |
| :-----------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|     Repository      |                        CrudRepository                        |                        JPARepository                         |
| Data flush Strategy | 명시적으로 플러시 및 커밋 메서드를 호출하지 않는 한 데이터베이스에 직접 데이터를 플러시하지 않는다. |          데이터를 데이터베이스에 직접 플러시합니다.          |
|      Bulk Save      |              CrudRepository에서 bulk를 제공함.               |                            불가능                            |
|         ex          | 동일한 트랜잭션의 나중에 저장된 변경 사항을 사용할 필요가 없을 때 이 방법을 사용한다. | 저장된 변경 사항을 나중에 동일한 트랜잭션에서 사용해야 할 때 이 방법을 사용합니다. |



> reference: https://www.baeldung.com/spring-data-jpa-save-saveandflush , https://www.tutorialspoint.com/difference-between-save-and-saveandflush-in-spring-java