---
title: HTTP 메소드(Method)의 종류와 속성
thumbnail: ./images/network/http.png
date: 2021-01-25 22:55:59
category: Computer Science
tags: [Network]
draft: false

---

# HTTP 메소드(Method)의 종류와 속성

개발자라면 당연히 알아야 되는 HTTP 메소드에 대해 정리를 해보았다. HTTP 메소드의 종류는 총 9가지가 있다.



## ⏬HTTP 메소드 종류

1. `GET`: URL에 해당하는 자료의 전송을 요청함.
   - 주로 조회할 때 사용
2. `POST`: 데이터 요청을 처리하고, 메시지 바디를 통해 서버로 데이터를 전달한다.
   - 주로 등록할 때 사용
3. `DELETE`: 해당 URL의 자료를 삭제한다.
4. `PATCH`: 자료의 부분만을 변경한다.
5. `PUT`: 자료를 대체한다, 해당 자료가 없으면 생성함.
   - `PATCH`와 달리 전체 필드 필요함.
6. `TRACE`: `TRACE` 메서드는 목적 리소스의 경로를 따라 메시지 loop-back 테스트를 한다.
7. `HEAD`: `GET`과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더(meta-information)만 반환
8. `OPTIONS`: 목적 리소스의 통신을 설정한다.
9. `CONNECT`: 목적 리소스로 식별되는 서버로의 터널을 맺는다.(프록시)



## ⚙HTTP 메소드 속성

1. 안전(Safe)
   - 계속해서 메소드를 호출해도 리소스를 변경하지 않는다.
2. 멱등(Idempotent)
   - 동일한 요청을 여러 번 보내도 한 번 보내는 것과 같은 것
3. 캐시가능(Cacheable)
   - 응답 결과를 서버에 캐시 해서 사용가능



| HTTP 메소드 | 안전 | 멱등 | 캐시가능 |
| ----------- | ---- | ---- | -------- |
| `GET`       | O    | O    | O        |
| `POST`      | X    | X    | O        |
| `DELETE`    | X    | O    | X        |
| `PATCH`     | X    | X    | O        |
| `PUT`       | X    | O    | X        |
| `TRACE`     | O    | O    | X        |
| `HEAD`      | O    | O    | O        |
| `OPTIONS`   | O    | O    | X        |
| `CONNECT`   | X    | X    | X        |



### 💡POST VS PUT

정리를 하다가 자연스레 들었던 의문이, `POST와 PUT의 본질적인 차이가 없지 않나?` 였다. `그냥 POST를 쓰지말고 PUT만 쓰면 되지 않을까?`

하지만 HTTP 메소드의 속성중에 **멱등**을 보면 POST는 만족하지 않는다. 왜냐하면, 100번 시도하면 100개의 리소스가 생성이 되기 때문이다. 반대로, PUT은 멱등을 만족한다. 즉 100번을 시도해도 하나의 결과만을 도출하는 것이다!!



> reference: https://developer.mozilla.org/ko/docs/Web/HTTP/Methods