---
title: JPA 기본 키 매핑
thumbnail: ./images/jpa/jpa.png
date: 2021-05-15 22:10:00
category: Back-End
tags: [Java, Spring, JPA, DB]
draft: false
---

# JPA 기본 키 매핑

프로젝트를 중 직접 할당방식을 쓰다가 `IDENTITY` 전략으로 바꾸게 되어서 스터디를 진행하며 공부한 것을 정리해보았다.



## 🔑 기본키 매핑 방식의 종류

JPA에서 기본 키를 생성하는 방식은 아래와 같다.

- 직접 할당: 기본 키를 애플리케이션에서 직접 할당한다.
- 자동 생성: 대리 키 사용 방식
  - `IDENTITY`: 기본 키 생성을 DB에 위임한다.
  - `SEQUENCE`: DB 시퀀스를 사용해서 기본 키를 할당 한다.
  - `TABLE`: 키 생성 테이블을 사용한다.



### 🛠 직접 할당 전략

___

```java
@Id
@Column(name = "id")
private String id;
```

```java
CustEntity custEntity = new CustEntity.Builder().id("example").build(); // 직접 할당

custRepository.save(custEntity);
```

기본 키 직접 할당 전략은 `em.persist()`로 엔티티를 저장하기 전에 애플리케이션에서 기본 키를 직접 할당하는 방법이다. 하지만 해당 방식은 Insert를 원하였는데 의도치 않은 Select 쿼리가 수행(중복PK발생)되어, Update가 되어버릴 수 있다.



### 🛠 IDENTITY 전략

___

주로 MySQL, PostgreSQL 등에 사용된다.

- ex) MySQL의 `AUTO_INCREMENT` 기능은 DB가 기본 키를 자동으로 생성해준다.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

```java
CustEntity custEntity = new CustEntity; // 할당할 필요 없음.

custRepository.save(custEntity);
```

이 전략은 DB에 Insert한 후에 기본 키 값을 조회 할 수 있다. 따라서 이 전략은 트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.



### 🛠 SEQUENCE 전략

___

DB 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트다. 이 전략은 이 시퀀스를 사용해서 기본 키를 생성한다.

주로, 오라클, H2 DB에서 사용할 수 있다.

```java
@Entity
@SequenceGenerator(
	name = "BOARD_SEQ_GENERATOR",
	sequenceName = "BOARD_SEQ", // 매핑할 데이터베이스 시퀀스 이름
	initialValue = 1, allocationSize = 1)
public class Board{
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE,
                    generator = "BOARD_SEQ_GENERATOR")
    private Long id;
}
```

SEQUENCE 전략은 먼저 데이터베이스 시퀀스를 사용해서 식별자를 조회한다. 그리고 조회한 식별자를 엔티티에 할당한 후에 엔티티를 영속성 컨텍스트에 저장한다. 이후 트랜잭션을 커밋해서 플러시가 일어나면 엔티티를 데이터베이스에 저장한다.



### 🛠 TABLE 전략

___

키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만들어 데이터베이스 시퀀스를 흉내내는 전략이다.

```sql
CREATE TABLE CUSTOM_SEQUENCE {
	sequence_name varchar(255) not null
    , next_val bigint
    , primary key (sequence_name)
}
```

```java
@Entity
@TableGenerator(
	name = "BOARD_SEQ_GENERATOR"
    , table = "MY_SEQUENCES"
    , pkColumnValue = "BOARD_SEQ"
    , allocationSize = 1
)
public class Board {

    @Id
    @GeneratedValue(
    	strategy = GenerationType.TABLE
    	, generator = "BOARD_SEQ_GENERATOR"
    )
    private long id;
    
}
```

TABLE 전략은 시퀀스 대신 테이블을 사용한다는 것만 제외하면 SEQUENCE 전략과 내부 동작방식이 같다.



> reference: http://www.yes24.com/Product/Goods/90439472
