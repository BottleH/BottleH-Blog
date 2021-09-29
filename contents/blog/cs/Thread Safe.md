---
title: Thread Safe란?
thumbnail: ./images/os.png
date: 2021-03-18 11:55:59
category: Computer Science
tags: [OS]
draft: false
---

# Thread-Safe
흔히 자연스럽게 쓰고 있고 자주 들어본 Thread-Safe의 정의와 이를 위한 방법들을 정리해보았습니다.



## Thread Safe란❓

[멀티 스레드 프로그래밍](https://bottleh.netlify.app/cs/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%8A%A4%EB%A0%88%EB%93%9C/)에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근되어도 프로그램의 실행에 문제가 없음을 뜻한다. 

- 하나의 함수가 한 스레드로부터 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행되더라도 **각 스레드에서의 함수의 수행 결과가 올바로 나오는 것**



## 🛠Thread Safe를 위한 방법

1. `Re-entrancy`

   - 재진입성

   - 어떤 함수가 한 스레드에 의해 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하더라도 그 결과가 각각에게 올바로 주어져야 한다.

2. `Thread local storage`

   - 스레드 지역 저장소

   - 공유 자원의 사용을 최대한 줄여 각각의 스레드에서만 접근 가능한 저장소들을 사용함으로써 동시 접근을 막는다.

3. `Mutual exclusion`

   - 상호 배제

   - Thread에 lock이나 semaphore를 걸어서 공유자원에는 하나의 thread만 접근 가능하게 한다.

4. `Atomic operations`

   - 원자연산

   - 데이터 변경시 `atomic`하게 데이터에 접근하도록 만든다.
     - `atomic`: 데이터가 반드시 변경전과 후의 상황에서만 접근하는 것

5. `Immutable Object`

   - 객체 생성 이후에 값을 변경할 수 없도록 만든다.
