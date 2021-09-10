---
title: JAR vs WAR
thumbnail: ./images/spring/spring.png
date: 2021-09-10 13:15:51
category: Back-End
tags: [Java, Spring]
draft: false
---

# JAR vs WAR

백기선님과의 런치 Spring Study 중에 나왔던 내용이었고, 저도 대충만 알고 있던 내용이라 정리해 보았다.



## ☄JAR

JAR(Java Archive)은 `.jar` 확장자를 가지며 라이브러리, 리소스 및 메타데이터 파일을 포함할 수 있다.

- `.class` 파일의 압축 버전과 컴파일된 Java 라이브러리 및 애플리케이션의 리소스를 포함하는 압축 파일

```text
META-INF/
    MANIFEST.MF
com/
    baeldung/
        MyApplication.class
```

위는 간단한 JAR 파일 구조이다.

META-INF / MANIFEST.MF의 파일은 아카이브에 저장된 파일에 대한 추가 메타 데이터가 포함되어있다.



## ☄WAR

WAR(Web Application Archive)은 웹 어플리케이션 리소스를 나타낸다. `.war` 확장자를 가지며 모든 Servlet/JSP 컨테이너에 배포할 수 있는 **웹 응용 프로그램을 패키징** 하는데 사용된다.

```text
META-INF/
    MANIFEST.MF
WEB-INF/
    web.xml
    jsp/
        helloWorld.jsp
    classes/
        static/
        templates/
        application.properties
    lib/
        // *.jar files as libs
```

HTML 페이지, 이미지 및 JS 파일을 포함한 모든 정적 웹 리소스 가 있는 *WEB-INF* 공용 디렉토리를 포함한다.



## 🔎 차이점

1. 파일 확장자
2. **목적과 작동방식**
   - JAR: 라이브러리, 플러그인 또는 모든 종류의 응용 프로그램으로 사용하기 위해 여러 파일을 패키징
   - WAR: 웹 응용 프로그램에만 사용
3. 아카이브의 구조
   - JAR: 원하는 구조로 만들 수 있다.
   - WAR: `WEB-INF` 및 `META-INF` 디렉토리 가 있는 사전 정의된 구조가 있다.

4. 실행
   - JAR: 명령줄에서 JAR 실행가능(실행 가능한 JAR로 빌드시)
   - WAR: 실행하려면 서버가 필요함.



> reference: https://www.baeldung.com/java-jar-war-packaging
