---
title: Spring Bean 과 Bean Scope
thumbnail: ./images/spring/spring.png
date: 2021-06-19 12:15:51
category: Back-End
tags: [Java, Spring]
draft: false
---

# Spring Bean 과 Bean Scope

Spring에서 흔히들 들어봤을 Bean의 정의와 더 나아가서 Bean Scope를 정리하고 이해해보자.



## ❓ Bean이란

Bean은 간단히 말하자면, `Spring IoC 컨테이너에 의해 관리되는 객체`이다.

✔ 왜 하필 이름이 bean일까

- Spring Framework는 Enterprise Java **Beans**(겨울)이 끝나고 봄이 왔다는 뜻으로 이름이 지어졌다. 이에 따라 Bean으로 지었다고 한다.(spring 공식 문서 참고)



## 🔎 Bean Scope

| Scope            | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `singleton`      | 하나의 bean 정의에 대해서 IoC Container 내에 단 하나의 객체만 존재한다.(default) |
| `prototype`      | 하나의 bean 정의에 대해서 다수의 객체가 존재할 수 있다.      |
| `request`        | 하나의 bean 정의에 대해서 하나의 HTTP request의 생명주기 안에 단 하나의 객체만 존재한다. |
| `session`        | 하나의 bean 정의에 대해서 하나의 HTTP Session의 생명주기 안에 단 하나의 객체만 존재한다. |
| `global session` | 하나의 bean 정의에 대해서 하나의 Global HTTP Session의 생명주기 안에 단 하나의 객체만 존재한다. |



좀더 나아가서, `singleton` 과 `prototype`의 차이에 대해 알아보자.



## ❗ singleton vs prototype

1. `singleton`
   - 싱글톤은 컨테이너에서 단 `한 번` 생성된다.
   - 생성된 인스턴스는 single beans cache에 저장된다.
   - bean의 기본 값이다.
2. `prototype`
   - 프로토타입은 모든 요청에서 **새로운** 객체를 반환한다.





> reference: https://docs.spring.io/spring-framework/docs/4.2.5.RELEASE/spring-framework-reference/html/beans.html#beans-factory-scopes
