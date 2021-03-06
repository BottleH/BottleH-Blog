---
title: 세션(Session)과 쿠키(Cookie)
thumbnail: ./images/network/cookie.png
date: 2021-01-10 23:58:59
category: Computer Science
tags: [Network]
draft: false
---

# 세션(Session)과 쿠키(Cookie)

세션과 쿠키는 둘다 웹 통신간 **유지 및 저장하려는 정보를 저장하기 위해** 사용하는 것들이다.



## 🖥 세션(Session)과 쿠키(Cookie) 사용이유

- HTTP 프로토콜의 특징이자 약점(connectionless, stateless)을 보완하기 위해서 사용

  - 💡 `connectionless`: 클라이언트가 요청을 한 후 응답을 받으면 그 연결을 끊어 버리는 특징
  - 💡 `stateless`: 통신이 끝나면 상태를 유지하지 않는다.



## ❓세션(Session)과 쿠키(Cookie)란

1. **세션**: 일정 시간동안 같은 사용자(브라우저)로부터 들어오는 일련의 요구를 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술
2. **쿠키**: 클라이언트의 컴퓨터에 저장하는 작은 기록 정보 파일



## ⚙세션(Session)과 쿠키(Cookie)의 차이점

|           | 세션(Session)   | 쿠키(Cookie)                                                 |
| --------- | --------------- | ------------------------------------------------------------ |
| 저장 위치 | 웹 서버         | 클라이언트                                                   |
| 저장 파일 | Object          | text                                                         |
| 만료      | 브라우저 종료   | 설정 가능                                                    |
| 용량제한  | 서버 허용수준   | 총 300개<br />하나의 도메인 당 20개 <br />하나의 쿠키 당 4KB |
| 속도      | 느림            | 빠름                                                         |
| 보안      | 상대적으로 좋음 | 상대적으로 좋지 않음.                                        |

