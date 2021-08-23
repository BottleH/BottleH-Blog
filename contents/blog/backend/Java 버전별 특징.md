---
title: Java 버전별 특징
thumbnail: ./images/java/java.png
date: 2021-08-23 20:53:00
category: Back-End
tags: [Java]
draft: false
---

# Java 버전별 특징

흔히 말하는 모던자바(8 이후 버전)의 **큰** 특징과 변경 및 추가사항들을 정리해보았다.



### ☄Java 1.8

- `Stream API`
- lambda expression
- `Optional`

- `LocalDateTime`
- 인터페이스 내 `default`, `static` 메소드 추가
- 32비트 마지막으로 지원



### ☄Java 9

- `HttpClient`
- 인터페이스 내 private 메서드 추가
- 모듈시스템 추가
- 프로세스 API: `java.lang.ProcessHandle`
- 버전 1.x 미사용



### ☄Java 10

- **지역 변수**에서 `var` 사용: 컴파일 타임 때 추론
- GC 병렬처리
  - 성능 향상
- JVM heap 영역을 **시스템 메모리가 아닌 다른 종류의 메모리**에도 할당 가능



### ☄Java 11

- lambda 지역변수 구문



### ☄Java 12

- `switch` 구문 문법적 확장

```java
switch (day) {
    case A -> System.out.println(10);
    case B                -> System.out.println(9);
    case C, D     -> System.out.println(8);
    case E              -> System.out.println(7);
}
```



> reference: https://www.baeldung.com/java-8-new-features, https://openjdk.java.net/jeps/325, [bryce님 블로그](![image-20210823202516957](C:\Users\김병환\AppData\Roaming\Typora\typora-user-images\image-20210823202516957.png))

