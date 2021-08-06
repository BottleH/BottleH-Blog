---
title: Java 묵시적 형변환(Promotion) vs 명시적 형변환(Casting)
thumbnail: ./images/java/java.png
date: 2021-02-18 15:15:51
category: Back-End
tags: [Java]
draft: false
---

# 묵시적 형변환(Promotion) vs 명시적 형변환(Casting)

Java에서 서로 다른 데이터 타입끼리 연산이 필요한 경우 **타입변환**이 필요하다. 타입변환의 종류에는 **묵시적 형변환(Promotion)**과 **명시적 형변환(Casting)**이 있다.



## 1. Promotion

Promotion은 자동으로 형변환이 일어나는 것이다.

- 작은 메모리 크기의 데이터 타입을 큰 메모리 크기의 데이터 타입으로 변환

```java
byte < short < int < long < float < double
       char  < int < long < float < double
```

여기서 주의할 점은 `byte` 와 `char` , `short`와 `char`은 변환되지 못한다. 타입 범위를 포함하지 못하기 때문이다.

- `short`와 `char`은 모두 2byte의 크기를 갖지만 서로 범위가 다르다.(`short`: -32768~32767, `char`: 0~65535)



```java
int bh = 10;
double bottle = bh;
```

위와 같이 작은 메모리 데이터 타입을 큰 메모리 데이터 타입으로 묵시적 형변환이 가능하다.



## 2. Casting

조건을 갖추지 못했지만, 형변환을 하고 싶을 때 명시적 형변환을 사용한다.

```java
double bottle = 5;
int bh = (int)bottle;
```

5라는 값은 `int`의 데이터 타입 범위 안에 충분히 들어간다. 그걸 알고 있기 때문에 Casting을 사용하는 것이다.

- int 범위가 아닐 수도 있기 때문에 java 진영에서 Promotion을 적용하지 않은 것임.



## 3. 연산

```java
int bh = 10;
double bottle = 15.5
// Promotion 수행 후 결과 저장
double result = bh + bottle;
// Casting 형변환 후 결과 저장
int = bh + (int)bottle;
```

서로 다른 데이터 타입의 피연산자가 있는 경우 큰 타입으로 Promotion 후 수행된다. 만약 작은 타입으로 연산 결과를 얻고 싶으면 Casting을 이용하자.



> Reference: [https://github.com/GimunLee/tech-refrigerator](https://github.com/GimunLee/tech-refrigerator/blob/master/Language/JAVA/Promotion%20%26%20Casting.md#promotion--casting)

