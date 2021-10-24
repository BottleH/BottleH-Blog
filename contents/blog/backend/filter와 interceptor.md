---
title: Spring Filter와 Interceptor
thumbnail: ./images/spring/spring.png
date: 2021-06-26 03:15:51
category: Back-End
tags: [Spring]
draft: false
---

# Spring Filter와 Interceptor



## 🛠Filter

J2EE 표준스펙인 필터(Filter)는 Dispatcher Servlet에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공한다. 

- 스프링 컨테이너가 아닌 톰캣과 같은 웹컨테이너(WAS)에 의해 관리가 되므로 디스패처 서블릿으로 가기 전에 요청을 처리하는 것
- HTTP Request → WAS → 필터 → 서블릿 → 컨트롤러



### Filter의 메소드

**init**

- 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드

- 웹 컨테이너가 1회 `init` 메소드를 호출하여 필터 객체를 초기화하면 이후의 요청들은 `doFilter`를 통해 전/후에 처리

 

**doFilter**

- url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전/후에 웹 컨테이너에 의해 실행

- `doFilter`의 파라미터로는 `FilterChain`이 있는데, `FilterChain`의 `doFilter` 통해 다음 대상으로 요청을 전달

 

**destroy**

- 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환

- 웹 컨테이너에 의해 1번 호출되며 이후에는 `doFilter`에 의해 처리되지 않는다.



## 🛠Interceptor

Spring이 제공하는 기술로써, 디스패처 서블릿(Dispatcher Servlet)이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공

- 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작을 하는 것이다.
- HTTP Request → WAS → 필터 → 서블릿 → 스프링 인터셉터 → 컨트롤러



### Interceptor의 메소드

**preHandle**

- 컨트롤러가 호출되기 전에 실행

**postHandle**

- 컨트롤러를 호출된 후에 실행

**afterCompletion**

- 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행



> reference: 토비의 스프링 vol2, [bryce님 블로그](https://bryceyangs.github.io/study/2021/10/04/Spring-%EC%84%9C%EB%B8%94%EB%A6%BF-%ED%95%84%ED%84%B0-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9D%B8%ED%84%B0%EC%85%89%ED%84%B0/)

