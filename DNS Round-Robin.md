# DNS Round Robin 방식

-----

> [toc]

<br/>

## 서버 부하분산 기술

> 3 종류

- **서버 부하분산 기술**이란 여러 대의 서버에 통신을 분배하여 처리 부하를 분산하는 기술이다.
- 부하분산 기술은 **DNS 라운드 로빈**, 서버 타입, 어플라이언스 서버타입 3종류로 나눌 수 있다.

<br/>

## DNS 라운드 로빈

> DNS 서버에

- DNS 라운드 로빈은 DNS을 이용하여 부하분산을 구현한다.
- DNS 서버에 하나의 도메인명에 여러 개의 IP 주소를 등록하고 클라이언트의 요청이 있을 시 IP 주소를 순서대로 반환하는 방식이다.

![img](/Volumes/Youngzi_T5/Dinoryong_GIT/Network/assets/server_load_balancing_technology_1.png)

<br/>

## DNS Round Robin 방식의 문제점

> 3가지

### 1. 공인 IP

- 서버의 수 만큼 공인 IP 주소가 필요함 부하 분산을 위해 서버의 대수를 늘리기 위해서는 그 만큼의 공인 IP 가 필요하다.

### 2. 균등하게 분산되지 않은 모바일 사이트 등

- 균등하게 분산되지 않음 모바일 사이트 등에서 문제가 될 수 있는데, 스마트폰의 접속은 캐리어 게이트웨이 라고 하는 프록시 서버를 경유 한다. 프록시 서버에서는 이름변환 결과가 일정 시간 동안 캐싱되므로 같은 프록시 서버를 경유 하는 접속은 항상 같은 서버로 접속된다. 또한 PC 용 웹 브라우저도 DNS 질의 결과를 캐싱하기 때문에 균등하게 부하분산 되지 않는다. DNS 레코드의 TTL 값을 짧게 설정함으로써 어느 정도 해소가 되지만, TTL 에 따라 캐시를 해제하는 것은 아니므로 반드시 주의가 필요하다.

### 3. 서버가 다운되도 확인이 불가한 DNS 서버

- 서버가 다운되도 확인 불가 DNS 서버는 웹 서버의 부하나 접속 수 등의 상황에 따라 질의결과를 제어할 수 없다. 웹 서버의 부하가 높아서 응답이 느려지거나 접속수가 꽉 차서 접속을 처리할 수 없는 상황인 지를 전혀 감지할 수가 없기 때문에 어떤 원인으로 다운되더라도 이를 검출하지 못하고 유저들에게 제공한다. 이때문에 유저들은 간혹 다운된 서버로 연결이 되기도 한다. DNS 라운드 로빈은 어디까지나 부하분산 을 위한 방법이지 다중화 방법은 아니므로 다른 S/W 와 조합해서 관리할 필요가 있다.

<br/>

## *Round Robin 방식을 기반으로 단점을 해소하는 DNS 스케줄링 알고리즘이 존재한다. (일부만 소개)*

> Weighted round robin, Least connection

#### Weighted round robin (WRR)

- 각각의 웹 서버에 가중치를 가미해서 분산 비율을 변경한다. 물론 가중치가 큰 서버일수록 빈번하게 선택되므로 처리능력이 높은 서버는 가중치를 높게 설정하는 것이 좋다.

#### Least connection

- 접속 클라이언트 수가 가장 적은 서버를 선택한다. 로드밸런서에서 실시간으로 connection 수를 관리하거나 각 서버에서 주기적으로 알려주는 것이 필요하다.

<br/>

Ref

- [Interview Question for Beginner](https://github.com/CS-box/Interview_Question_for_Beginner/tree/master/Network#dns-round-robin-%EB%B0%A9%EC%8B%9D)

- [Goodgid blog - DNS](https://goodgid.github.io/NW-DNS/)

  [DNS 서버의 역할](https://goodgid.github.io/Server-DNS/)

  [DNS 서버의 이중화](https://goodgid.github.io/Server-DNS-Redundancy/)

  [서버 부하분산 기술](https://goodgid.github.io/Server-Server-Load-Balancing-Technology/)

- [Youtube - 10분 테코톡 - 동글 & 라면의 DNS](https://youtu.be/5rBzHoR4F2A)

- [Youtube - DNS 구성요소 및 동작원리](https://youtu.be/fINh76spaiI)

