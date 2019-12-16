---
layout: post
title:  " Kafka 메시지 손실 (Under Certain Conditions) "
categories: Kafka
tags: Kafka
author: goodGid
---
* content
{:toc}

> 이 글의 코드 및 정보들은 [책](https://book.naver.com/bookdb/book_detail.nhn?bid=13540082)을 바탕으로 작성하였습니다.

## 메시지 손실

* acks = 1 옵션으로 

* 프로듀서가 카프카 토픽의 리더에게 메시지를 전송했는데

* 메시지가 손실되는 

* 아주 예외적인 상황에 대해 알아보자.

---

## 정상적인 상황

* 우선 정상적인 상황에 대해 알아보자.

![](/assets/img/kafka/Kafka-Send-Message-Mehotd_3.png)

```
1. 프로듀서가 acks = 1으로 토픽의 리더에게 메시지를 보낸다.

2. 리더는 메시지를 받은 후 저장한다.

3. 리더는 프로듀서에게 메시지를 받았다고 acks를 보낸다.

4. 팔로워들은 리더를 주기적으로 체크한다.

5. 리더에 새로운 메시지가 있는 것을 확인하고 

   팔로워들도 저장한다.
```





* 일반적인 상황에서는

* 메시지 손실 문제가 발생하지 않는다.


---

## 예외 상황

* **리더에 장애가 발생**하는 순간

* 메시지 손실이 발생하게 된다.

![](/assets/img/kafka/Kafka-Send-Message-Mehotd_4.png)

* 정상적인 상황처럼

* 리더가 프로듀서에게 보낸 메시지를 잘 받았다고

* acks를 보내는 3번까지의 작업은 진행된 상태이다.

* 그리고 예외적인 상황에 대해서는 

* (= 4번 작업 이후)

* **점선**으로 표시되어 있다.

```
1. 프로듀서가 acks = 1으로 토픽의 리더에게 메시지를 보낸다.

2. 리더는 메시지를 받은 후 저장한다.

3. 리더는 프로듀서에게 메시지를 받았다고 acks를 보낸다.

4. 팔로워들은 리더를 주기적으로 체크해야하는데

   리더가 없는 상태이다.

5. 리더에 새로운 메시지가 있는지 모르기 때문에

   메시지를 가져올 수 없다.
```

![](/assets/img/kafka/Kafka-Send-Message-Mehotd_5.png)

* 요약해보자면

* 프로듀서가 전송한 메시지는

* 브로커2에만 저장되어 있는 상태이고

* 팔로워들은 리더가 갑작스레 다운되면서

* 해당 메시지를 가지고 있지 않다.

<br>

* 카프카에서는

* 리플리케이션 동작 방식에 따라

* 리더가 다운되었기 때문에

* 팔로워 중 하나가 새로운 리더가 되고

* 프로듀서의 요청을 처리하게 된다.

<br>

* 즉 팔로워들은 브로커2로부터

* 장애 발생 직전의 메시지는 가져오지 못하였지만

* 그 상태 그대로 새로운 리더가 되는 것이다.

---

## Summary

* 프로듀서 / 리더 / 팔로워

* 모두 정의된 프로세스에 따라 행동하였다.

* 하지만 프로듀서가 acks = 1로 보낸 메시지는 손실되었다.

* 이것이 바로 프로듀서 acks = 1로 설정했을 때 

* 메시지가 손실하는 아주 예외적인 경우이다.

---

## 참고

* [카프카, 데이터 플랫폼의 최강자 실시간 비동기 스트리밍 솔루션 카프카의 기본부터 확장 응용까지](https://book.naver.com/bookdb/book_detail.nhn?bid=13540082)