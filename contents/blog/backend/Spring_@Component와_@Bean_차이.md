---
title: Spring @Component와 @Bean 차이
thumbnail: ./images/spring/spring.png
date: 2021-06-18 03:15:51
category: Back-End
tags: [Spring]
draft: false
---

# Spring @Component와 @Bean 차이

프로젝트에서 Spring Batch를 이용한 개발을 하다가 `@Component`와 `@Bean`의 차이가 궁금해서 간단하게 정리했다.



|      | `@Component`                                                 | `@Bean`                                                      |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 용도 | **직접** 컨트롤이 가능한 Class들을 Bean으로 등록하고 싶은 경우 | 개발자가 컨트롤이 **불가능한** 외부 라이브러리들을 Bean으로 등록하고 싶은 경우 |
| 타입 | Class                                                        | Method                                                       |

