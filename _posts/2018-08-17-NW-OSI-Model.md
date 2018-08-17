---
layout: post
title:  " [OSI 참조 모델과 TCP/IP 기초] OSI 참조 모델 "
date:   2018-08-17
excerpt: " OSI 참조 모델 "
cate : "posts"
tag:
- Network
---

# OSI 참조 모델

* 제 1계층 - 물리 계층 <br> 케이블의 핀 수, 전기 특성을 정하고 송출 데이터의 전기적진 변환 등을 수행

* 제 2계층 - 데이터링크 계층 <br> 직접 연결되는 상대와의 통신로를 확보, 데이터 오류 정정 or 재송신 요구 등 수행

* 제 3계층 - 네트워크 계층 <br> 데이터를 상대가 있는 곳까지 전달하기 위한 경로 선택, 네트워크에서 각각을 식별하기 위한 주소 관리

* 제 4계층 - 전송 계층 <br> 네트워크 계층에서 보내져 온 데이터의 정렬과 오류 정정 등 수행, 송수신된 데이터의 신뢰성 확보, TCP/UDP 프로토콜 존재

* 제 5계층 - 세션 계층 <br> 통신의 지삭과 종료와 같은 통신 프로토콜 간의 연결 관리, 통신 경로 확립

* 제 6계층 - 표현 계층 <br> 압축 방법, 문자 인코딩을 관리, 애플리케이션 SW와 네트워크와의 중개 수행

* 제 7계층 - 응용 계층 <br> 통신을 이용하기 위해 필요한 서비스를 사람과 다른 컴퓨터에 제공