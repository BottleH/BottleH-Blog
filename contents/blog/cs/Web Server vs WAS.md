---
title: Web Server vs WAS
thumbnail: ./images/webwas.png
date: 2021-02-16 18:55:59
category: Computer Science
tags: [web server, was]
draft: false
---

# Web Server vs WAS
Web Server와 Web Application Server의 차이점을 알아보자!



## 1. Web Server

Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받아 **정적컨텐츠**(이미지, html 문서)를 제공하는 어플리케이션이다.

- WAS를 거치지 않고 바로 자원을 제공함.
- 동적 컨텐츠 제공을 위한 요청을 전달한다.
  - 웹 브라우저 클라이언트로부터 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 응답(Response)
- ex) Apache Server, Nginx, IIS 등



## 2. Web Application Server

- Web Server + Web Container
  - Web Container는 서블릿 컨테이너라고도 불린다.
  - Web Container는 jsp, Servlet을 실행시킬 수 있다.
- jsp, php, DB조회 등의 **동적 컨텐츠** 처리
- DB 접속기능 제공
- 여러 트랜잭션 관리
- ex) Tomcat, JBoss, Web Sphere 등
- WAS가 없다면 Web Server는 원하는 요청에 대한 결과값을 미리 만들어놓아야함. 하지만 WAS로 인해 그때 그때 들고올 수 있다.

