---
title: JPA Auditing
thumbnail: ./images/jpa/jpa.png
date: 2022-02-13 16:53:51
category: Back-End
tags: [Java, Spring, JPA]
draft: false
---

# JPA Auditing
대부분의 테이블은 공통적으로 관리되는 시스템생성일자, 시스템생성자.. 등의 컬럼이 존재합니다. JPA를 적용할 때, Entity Class에 해당 컬럼을 계속해서 선언할 필요가 있을까요? 그래서 Spring Data JPA에서 Auditing 기능을 제공합니다.

## 01. Entity

```java
@Entity
@Table(name = "member")
public class MemberEntity {

    @Id
    @Column(name = "ID")
    private Long id;

    @Column(name = "NAME")
    private String name;

    @Column(name = "SYS_REG_DTM")
    private LocalDateTime sysRegDtm;

    @Column(name = "SYS_REGR_ID")
    private String sysRegrId;

    @Column(name = "SYS_MDFY_DTM")
    private LocalDateTime sysMdfyDtm;

    @Column(name = "SYS_MDFYR_ID")
    private String sysMdfyrId;
}

```

Member Entity에 Audit 기능을 적용해보겠습니다.

## 02. @EnableJpaAuditing

```java
@EnableJpaAuditing
@SpringBootApplication
public class BlogcodeApplication {

    public static void main(String[] args) {
        SpringApplication.run(BlogcodeApplication.class, args);
    }

}
```

스프링 부트 실행 클래스에 `@EnableJpaAuditing`을 설정해줍니다.

## 03. Audit 추상화 클래스

이 컬럼들을 모든 Entity에 적용시켜주기 위해 추상화 클래스를 작성합니다.

```java
@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
@Getter
public class Audit {

    @CreatedDate
    @Column(name = "SYS_REG_DTM", updatable = false)
    private LocalDateTime sysRegDtm;
    
    @CreatedBy
    @Column(name = "SYS_REGR_ID")
    private String sysRegrId;

    @LastModifiedDate
    @Column(name = "SYS_MDFY_DTM")
    private LocalDateTime sysMdfyDtm;

    @LastModifiedBy
    @Column(name = "SYS_MDFYR_ID")
    private String sysMdfyrId;
}
```

- `EntityListeners`:  엔티티를 DB에 적용하기 전, 이후에 **커스텀 콜백**을 요청할 수 있는 어노테이션
  - 여기서는 Auditing을 수행하기 위해 `AuditingEntityListener`를 인자로 넘깁니다.
- `MappedSuperClass`: Audit Class의 속성만 받아서 사용하기 위한 어노테이션
- `CreateDate`: 엔티티가 생성될 때, 생성하는 시각을 자동으로 삽입
- `CreatedBy`: 엔티티가 생성될 때, 생성하는 사람이 누구인지 자동으로 삽입
  - `AuditorAwareConfig` 필요
- `LastModifiedDate`: 엔티티가 수정될 때, 수정시각을 자동으로 삽입
- `LastModifiedBy`: 엔티티가 수정될 때, 수정하는 사람이 누구인지 자동으로 삽입
  - `AuditorAwareConfig` 필요

## 04. AuditorAwareConfig 클래스

```java
@Component
public class AuditorAwareConfig implements AuditorAware<String> {
    
    @Override
    public Optional<String> getCurrentAuditor() {
        return Optional.of("BOTTLEH");
    }
}
```

생성자, 수정자를 넣어주기 위해 `AuditorAwareConfig ` Class 를 작성합니다.



## 05. extends Audit

```java
@Getter
@Entity
@Table(name = "member")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class MemberEntity extends Audit {

    @Id
    @Column(name = "ID")
    private Long id;

    @Column(name = "NAME")
    private String name;

    @Builder
    public MemberEntity(Long id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

Entity Class에 Audit을 상속받아 줍니다.



위와 같이 Audit 기능을 사용하면 Create와 Update 시, 따로 값을 넣어주지 않아도 셋팅이 됩니다.

모든 코드는 [Github](https://github.com/BottleH/BlogCode/blob/main/src/main/java/com/bottleh/blogcode/common/audit/Audit.java)에 있습니다.