# DNS

---

> [toc]

<br/>

> 클라이언트와 서버, 도메인, TCP/IP, IP 주소

<br/>

## DNS 의미

> 영어 약자,  네트워크의 전화번호부

- Domain Name System

- 네트워크에서 호스트명(컴퓨터명)으로부터 대응하는 IP 주소를 검색하여 취득하는 서비스를 말한다.

  (DNS란 이른바 네트워크에서의 **전화번호부**와 같은 것이라 생각할 수 있다.)

- DNS 서버 안에서는 호스트명과 IP 주소를 대응시킨 **DB**를 관리하고 있어
  조회 시 서버는 DB에서 검색하여 호스트명에 해당하는 IP 주소를 반환한다.

![DNS](/Volumes/Youngzi_T5/Dinoryong_GIT/Network/assets/DNS.jpg)

<br/>

## DNS 탄생 배경

> IP, hosts, SRI

- IP
- hosts
- SRI



<br/>

## DNS 서버의 역할

> IP 주소, 도메인명

- 인터넷에서는 컴퓨터를 식별하기 위해 IP 주소를 사용
- 하지만 이 숫자만으로는 무엇에 사용되고 있는지 알 수 없다.
- 그래서 인터넷에서는 IP 주소에 `도메인명`이라는 이름을 붙혀 알기 쉽게 한다.
- IP 주소와 도메인명을 서로 교환하는 장치를 `DNS(Domain Name System)`이라 한다.

<br/>

## DNS 서버의 이중화

> 이름 해결

- DNS 서버는 인터넷을 보이지 않는 곳에서 지지해 주는 중요한 서버이다.
- DNS 서버에서 도메인명을 **[이름 해결](https://goodgid.github.io/Server-DNS/#dns-서버는-2종류)**할 수 없으면 목적하는 웹 서버에 Access할 수 없다.
- 그래서 DNS 서버는 단독 구성이 아니라 `프라이머리 DNS 서버`와 `세컨더리 DNS 서버`와 같이 `이중 구성`으로 구축하는 것이 기본이다.

<br/>

## 캐시 서버의 이중화

> LAN, 프라이머리 DNS 서버, 세컨더리 DNS 서버

- LAN에 배치하는 캐시 서버는 클라이언트가 조회한 이름 해결 정보를 캐시할 뿐이다.
- 따라서 프라이머리 DNS 서버와 세컨더리 DNS 서버에서 특별한 이중화 설정을 할 필요가 없다.
- 프라이머리 DNS 서버로부터 Response가 오지 않으면 세컨더리 DNS에게 다시 조회한다

<br/>

## 콘텐츠 서버의 이중화

> zone 파일, 동기화하는 때

- 콘텐츠 서버는 도메인명에 관한(zone 파일)을 저장하는 중요한 서버이다.
- 만일 프라이머리 DNS 서버가 다운되어도 세컨더리 DNS 서버가 동일한 정보를 반환할 수 있도록
  동일한 `zone 파일`을 저장해 둘 필요가 있다.
- 이것은 프라이머리 DNS 서버에서 세컨더리 DNS 서버로 zone 전송을 하여 `zone 파일`을 `동기화`한다.
- 정기적 or 임의의 타이밍에 zone 파일을 동기화한다.



<br/>



## DNS 통신 구조

> 과정, 구성 요소

- 간단한 통신 과정
- DNS 통신 구성 요소

<br/>

## DNS 에서의 질의와 응답

> 여러 객체들 사이의 협력으로 이루어진다.

- ㄹㄴㅇ
- ㄴㄹ

<br/>

## DNS 리소스 레코드

> ㄹ

- ㄹㄴㅇ
- 

<br/>

## DNS 서버의 종류

> 2종류 : 캐시 서버, 콘텐츠 서버

- DNS 서버는 `캐시 서버`와 `콘텐츠 서버`로 크게 나눈다.
- `캐시 서버` 는 LAN 안에 있는 클라이언트로부터 조회를 받아 클라이언트를 대신하여 인터넷에 조회해 주는 DNS 서버
  - 클라이언트가 인터넷에 Access할 때 사용
- `콘텐츠 서버` 는 외부 호스트로부터 자신이 관리하는 도메인에 관한 조회를 받는 DNS 서버
  - 자신의 도메인 내의 호스트명은 `zone 파일`이라는 DB에서 관리
- 클라이언트로부터 조회를 받은 캐시 서버는 받은 도메인명을
  `오른쪽`부터 순서대로 검색하여 해당 도메인명을 관리하는 콘텐츠 서버를 찾는다.
  거기까지 도달하면 해당 콘텐츠 서버에 대해 `호스트명+도메인명`에 대응하는 IP주소를 가르쳐 준다.
  이러한 동작을 `이름 해결`이라 한다.

<br/>

## 권한 없는 서버 - Public DNS

> fsf

- fsdf
- sfd

<br/>

## 도메인 네임

> 트리 구조

- 도메인명은 ‘[www.examaple.co.kr’과](http://www.examaple.co.xn--kr-o2tu607e/) 같이 점으로 구분된 문자열로 구성되어 있다.
- 이 하나하나의 문자열을 `라벨`이라고 하며,
  `오른쪽`부터 순서대로 ‘탑레벨 도메인’, ‘제 2레벨 도메인’, ‘제 3레벨 도메인’과 같이 부른다.
- 즉 트리 모양의 계층 구조로 되어 있다.

<br/>

## 권한 있는 서버 - 계층형 Name Server

> fsd

- 
- fs

<br/>

## 

<br/>

### Ref

- Textbook

  [그림 한 장으로 정리하는 최신 네트워크 용어 해설](https://github.com/Dinoryong/Network/tree/main/NETWORK-TERMS)

- Blog

  [Goodgid blog - DNS](https://goodgid.github.io/NW-DNS/)

  [DNS 서버의 역할](https://goodgid.github.io/Server-DNS/)

  [DNS 서버의 이중화](https://goodgid.github.io/Server-DNS-Redundancy/)

  [서버 부하분산 기술](https://goodgid.github.io/Server-Server-Load-Balancing-Technology/)

- Youtube

  [10분 테코톡 - 동글 & 라면의 DNS](https://youtu.be/5rBzHoR4F2A)

