---
title: 스프링과 스프링부트 차이(Spring vs Spring Boot)
thumbnail: ./images/spring/spring.png
date: 2021-06-17 15:15:51
category: Back-End
tags: [Java, Spring]
draft: false
---

# Spring vs Spring Boot

Spring과 Spring Boot의 차이를 간단히 정리해 보자.



## 1. 개요

![Spring vs Spring Boot](.images/../images/spring/spring_springboot.png)

현재 비교하고자 하는 Spring이라는 단어는 **Spring Framework**이다. 물론 실무에서는 Spring Boot, Spring 생태계 전체 등 여러가지 의미로 혼합되어 쓰이곤 한다. Spring Boot는 **Spring Framework의 모듈**이다!



## 2. 차이점

| Spring                                                       | Spring Boot                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 서버 설정 필요                                               | Tomcat, Jetty 등 임베디드 WAS서버 제공                       |
| `pom.xml` 에서 Spring 프로젝트에 대한 종속성을 수동으로 정의 | Spring Boot Requirement를 기반으로 하는 종속성 **JAR** 다운로드를 내부적으로 처리하는 `pom.xml` 파일 의 **starter** 개념과 함께 제공 |
| In Memory DB에 대한 미지원                                   | In Memory DB에 대한 지원                                     |
| 수동설정                                                     | `@EnableAutoConfiguration` 선언을 통한 자동설정              |
