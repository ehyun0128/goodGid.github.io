---
layout: post
title:  " 파일 입출력 제어 [ Part 2 ] "
date:   2018-04-15
excerpt: " 파일 입출력 제어 [2] "
cate : "posts"
tag:
- File Processing
---

# `[1] 단순 버퍼 시스템`

--- 

# 버퍼를 채우는 채널 프로그램의 기본 구성

1. 프로그램의 READ 명령이 있을 때까지 기다림

2. 제어 장치에 I/O시작 명령을 내림

3. 버퍼가 채워지기를 기다림

4. 프로그램에 인터럽트를 발생시켜 버퍼로부터 데이터를 읽도록 함 <br> --> 사용자 프로그램은 버퍼가 찰 때까지 유휴 상태


---

# 예상 버퍼링

* 프로그램이 대기 상태에 있을 가능성을 어느 정도 제거

* I/O 제어 시스템은 <br> 프로그램이 필요로 할 데이터를 미리 예측해서 <br> 항상 버퍼를 가득 채워 놓음 <br> --> 프로그램은 버퍼가 채워질 때까지 기다릴 필요 X (= 필요할것 같다 싶으면 미리 Get)


---

# 단순 버퍼 시스템의 단점 

1. 프로그램이 매 n+1번 째 read를 요청할 때마다 <br> 응용 프로그램, 즉 CPU는 버퍼가 다 채워질 때까지 대기 상태

2. 버퍼가 비워지는 동안 `생산자`는 버퍼를 채우기 위해 `대기 상태`

3. 그러므로 다중버퍼를 사용한다.


---

<br>

<br>

<br>

# `[2] 이중 버퍼 시스템`

* 파일당 두 개의 버퍼 구역을 할당 <br> 하나를 비우는 동안 나머지 버퍼를 채움

---

# 이중 버퍼 시스템의 장/단점

## 장점

* 버퍼를 채우고 비우는 작업이 중복되어 `병렬`로 수행되므로 <br> `CPU`의 `대기 시간` 감소

## 단점

* 생산자나 소비자 루틴의 `실행 시간`과 `복잡성` 증가 

* `메인 메모리`의 `소요량` 증가
