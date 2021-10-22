---
title: 아파치 카프카(Apache Kafka)의 용어 및 특징
thumbnail: ./images/bigdata/kafka.png
date: 2021-05-10 15:55:59
category: DevOps
tags: [BigData, Kafka]
draft: false
---

# 아파치 카프카란?
카프카는 링크드인에서 개발한 **대용량, 대규모 메시지 처리 시스템**이다.



## ⚙Pub-Sub 구조

카프카는 `Publisher` / `Subscriber` 모델을 사용한다. publisher는 메세지를 topic을 통해서 카테고리화하고, 해당 topic을 subscribe 함으로써 메세지를 읽어 올 수 있다. 자 그렇다면, Kafka의 용어에 대해서 알아보자.



## 🛠카프카의 용어 

- `broker` : 메시지 브로커 서버
  - 동일한 노드 내에서 여러 개의 브로커 서버를 띄울 수 있다.
- `topic` : `producer`와 `consumer`가 데이터를 담은 Que
- `partition` : topic을 분할하는 단위
  - 한개의 파티션은 최대 한개의 consumer에게 할당 가능하다.
  - 대용량 처리를 위해 분할하는 것이다.
  - Round Robin 방식으로 저장(순차적으로 쓰여지지 않음.)
- `producer` : topic에 메시지를 발행하는 주체
- `consumer` : topic의 메시지를 구독하는 주체
  - offset을 통한 읽은 위치 관리
- `offset` : topic의 partition마다 할당된 **consumer가 현재까지 읽은 위치에 대한 정보**를 offset으로 저장한다.
- `comsumer group` : 특정 컨슈머들의 묶음
  - **반드시 해당 topic의 파티션은 그 consumer group과 1:n 매칭을 해야한다.**
- `rebalance` : 컨슈머 그룹내에서 컨슈머들의 파티션 소유권 조정을 하는 작업
- `replication factor` : 하나의 토픽을 몇개의 브로커에 분산 저장할지 결정하는 factor
- `ACK` : producer 상세옵션
  - **0** : producer가 토픽을 leader에 전송 후 반환값을 받지않는다.
  - **1** : producer가 토픽을 leader에 전송 후 리더의 정상처리만 확인한다.
  - **ALL** : producer가 토픽을 leader에 전송 후 리더 및 팔로워 모두의 정상처리를 확인한다.



> reference: https://kafka.apache.org/documentation/