---
title: 02.Client Side Load Balancer - Ribbon Client
thumbnail: ./images/MSA/msa.png
date: 2021-10-05 15:55:59
category: DevOps
tags: [MSA, Spring]
draft: false
---

# 02.Client Side Load Balancer - Ribbon Client
이전 포스팅에 이어, `Client Side Load Balacer` 인 `Ribbon Client`에 대해 다뤄보겠습니다.



## ☄Server Side Load Balancer vs Client Side Load Balancer

MSA를 활용하기 전 우리가 흔히 사용하는 로드 밸런서는 Server Side Load Balancer인 L4 Switch(하드웨어 장비) 였다. 하지만 MSA에서 L7 Layer에서 동작하는 Client Side Load Balancer(소프트웨어)를 이용한다.

1. 하드웨어 기반 Server Side Load Balancer의 단점
   - 별도의 장비 필요
   - 비싸다!!
   - 서버목록 추가 등이 **수동**으로 이루어짐



## 🛠Ribbon Client

### 1. 개요

Netflix OSS 라이브러리 중 Hardware적인 Load Balancer를 대신해 L7 Layer에서 **Client Side Load Balacer** 역할을 담당한다.

- 최근 `RestTemplate` 대신 사용하는 `FeignClient`의 경우 이미 Ribbon 기능이 포함되어 있음.



### 2. 장점

- REST API를 호출하는 서비스에 탑제되는 SW 모듈
- 주어진 서버 목록에 대해 Load Balancing 수행
- 매우 다양한 설정이 가능(서버선택, 실패시 Skip시간, Ping체크 등등)
- Ribbon에는 `Retry` 기능이 내장되어 있음.
  -  응답을 받지 못하였을 경우 동일한 서버로 재시도 하거나 다른 서버로 재시도하는 기능
- Eureka와함께 사용되어 매우 강력한 기능 발휘



개인적인 생각으로는 유연함 하나만으로도 MSA에서는 필수적인 로드밸런서라는 생각이 듭니다.



> reference: https://github.com/Netflix/ribbon/wiki/Getting-Started
