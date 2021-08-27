---
title: Spring Data JPA에서 save 와 saveAll 비교
thumbnail: ./images/jpa/jpa.png
date: 2021-08-27 18:10:00
category: Back-End
tags: [Java, Spring, JPA]
draft: false
---

# Spring Data JPA에서 save 와 saveAll 비교

프로젝트를 중 다건 데이터를 업데이트 하기 위해, 자연스럽게 `saveAll` 메서드를 쓰고 있었다. 그런데 문득 `save`와 `saveAll` 메서드의 차이가 궁금해져 정리해 보았다.



## 🔎 예시코드

https://www.baeldung.com/spring-data-save-saveall 을 참고하여 만들어보았다.

### 01. Entity

```java
@Entity
@Getter
public class UserEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long userId;    
    private String userNm;
    
    public void updateUserNm(String userNm){
        if(userNm != null){
            this.userNm = userNm;
        }
    }
}
```

- `updateUserNm`을 사용하여 `Entity` 의 `userNm` 필드 업데이트

### 02. Repository

```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```



### 03. Service

```java
private UserRepository userRepository;

// 단건 업데이트 메소드
public void updateUser(long userId, String userNm){
    
    List<UserEntity> userEntitys = userRepository.findAllById(userId); // DB에서 userId 조회하여 영속성 컨텍스트에 넣어준다.
    
    for(UserEntity user : userEntitys){
        user.updateUserNm(userNm) // 사용자이름 업데이트
        userRepository.save(user); // 저장
    }
}

// 다건 업데이트 메소드
public void updateUserList(long userId, String userNm){
    
    List<UserEntity> userEntitys = userRepository.findAllById(userId); // DB에서 userId 조회하여 영속성 컨텍스트에 넣어준다.
    
    for(UserEntity user : userEntitys){
        user.updateUserNm(userNm) // 사용자이름 업데이트
    }
    
    userRepository.saveAll(userEntitys); // 저장
}
```



### 04. 비교

약 1만건의 데이터로 비교결과 **거의 2배** 가까이 `saveAll()` 메서드 성능이 좋은 것을 알 수 있다.



## ❓ 왜 이런 차이가 발생할까?

`saveAll` 메서드의 내부 구현은 loop로 `save()`를 사용한다. 그렇다면 성능이 똑같아야 하지 않을까?



두 메서드의 내부 구현은 `@Transactional`이 선언되어 있다. 따로 옵션이 설정되어 있지 않아 기본 전파 유형인 `REQUIRED`로 전파된다.

- `REQUIRED`: 제공하지 않으면 메서드가 호출될 때마다 새 트랜잭션이 생성됨.

**즉, `save()` 메소드를 호출할 때마다 새로운 트랜잭션이 생성되는 반면, `saveAll()` 을 호출 하면 하나의 트랜잭션만 생성되기 때문에 성능 차이가 생긴다.**



> reference: https://www.baeldung.com/spring-data-save-saveall



### 
