# Network

------

> 
>
> [toc]

<br/>

<br/>

## Index

- HTTP 의 Method : GET vs POST
- HTTP & HTTPS
- HTTP 1.1 ~ 2.0
- 대칭키 & 공개키
- Cookie-session
- REST-api
- TCP vs UDP
- TCP 3-way-handshake
- DNS Round Robin
- 흐름제어 & 혼잡제어 & 오류제어
- 웹 통신의 큰 흐름 (type-url-process) , HTTP를 사용한 통신 과정
- OSI 7 계층
- Blocking Non-Blocking IO
- 로드 밸런싱 (Load Balancing)
- cdn
- URI
- CORS
- HTTP Keep Alive
- OSI 참조 모델과 TCP/IP 기초
  - IP 주소
  - IP
  - 노드
  - OSI 참조 모델
  - 패킷
  - 서브넷 마스크
  - TCP / IP
- HTTP vs Socket
- 로컬영역 네트워크편
  - Ethernet
  - Token Ring
  - 무선 LAN
- 와이드 영역 네트워크
  - VPN
- 하드웨어
  - 브리지
  - 게이트웨이
  - 리피터
  - 라우터
- 서비스 프로토콜
  - DHCP
  - DNS
  - NAT
  - 프록시 서버
- 인터넷편
  - ISP
  - Java
  - RSS
  - SOAP
  - URL
- 네트워크 주소에 대한 모든 것
- 다양한 Timeouet을 알아보자
  - Connection Timeout
  - Socket Timeout
  - Read Timeout
- 포워드 Forward 프록시와 리버스 Reverse 프록시
- 네이버 메인 페이지의 트래픽 처리

<br/>

## HTTP 의 GET vs POST

> fsdfsd

둘 다 HTTP 프로토콜을 이용해서 서버에 무엇인가를 요청할 때 사용하는 방식이다. 하지만 둘의 특징을 제대로 이해하여 기술의 목적에 맞게 알맞은 용도에 사용해야한다.

### GET

우선 GET 방식은 요청하는 데이터가 `HTTP Request Message`의 Header 부분에 url 이 담겨서 전송된다. 때문에 url 상에 `?` 뒤에 데이터가 붙어 request 를 보내게 되는 것이다. 이러한 방식은 url 이라는 공간에 담겨가기 때문에 전송할 수 있는 데이터의 크기가 제한적이다. 또 보안이 필요한 데이터에 대해서는 데이터가 그대로 url 에 노출되므로 `GET`방식은 적절하지 않다. (ex. password)

### POST

POST 방식의 request 는 `HTTP Request Message`의 Body 부분에 데이터가 담겨서 전송된다. 때문에 바이너리 데이터를 요청하는 경우 POST 방식으로 보내야 하는 것처럼 데이터 크기가 GET 방식보다 크고 보안면에서 낫다.(하지만 보안적인 측면에서는 암호화를 하지 않는 이상 고만고만하다.)

*그렇다면 이러한 특성을 이해한 뒤에는 어디에 적용되는지를 알아봐야 그 차이를 극명하게 이해할 수 있다.* 우선 GET 은 가져오는 것이다. 서버에서 어떤 데이터를 가져와서 보여준다거나 하는 용도이지 서버의 값이나 상태 등을 변경하지 않는다. SELECT 적인 성향을 갖고 있다고 볼 수 있는 것이다. 반면에 POST 는 서버의 값이나 상태를 변경하기 위해서 또는 추가하기 위해서 사용된다.

부수적인 차이점을 좀 더 살펴보자면 GET 방식의 요청은 브라우저에서 Caching 할 수 있다. 때문에 POST 방식으로 요청해야 할 것을 보내는 데이터의 크기가 작고 보안적인 문제가 없다는 이유로 GET 방식으로 요청한다면 기존에 caching 되었던 데이터가 응답될 가능성이 존재한다. 때문에 목적에 맞는 기술을 사용해야 하는 것이다.

<br/>





<br/>

## HTTP vs HTTPS

> fsdfd

### HTTP 의 문제점

- HTTP 는 평문 통신이기 때문에 도청이 가능하다.
- 통신 상대를 확인하지 않기 때문에 위장이 가능하다.
- 완전성을 증명할 수 없기 때문에 변조가 가능하다.

*위 세 가지는 다른 암호화하지 않은 프로토콜에도 공통되는 문제점들이다.*

### TCP/IP 는 도청 가능한 네트워크이다.

TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있다. 패킷을 수집하는 것만으로 도청할 수 있다. 평문으로 통신을 할 경우 메시지의 의미를 파악할 수 있기 때문에 암호화하여 통신해야 한다.

#### 보안 방법

1. 통신 자체를 암호화 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 다른 프로토콜을 조합함으로써 HTTP 의 통신 내용을 암호화할 수 있다. SSL 을 조합한 HTTP 를 `HTTPS(HTTP Secure)` or `HTTP over SSL`이라고 부른다.
2. 콘텐츠를 암호화 말 그대로 HTTP 를 사용해서 운반하는 내용인, HTTP 메시지에 포함되는 콘텐츠만 암호화하는 것이다. 암호화해서 전송하면 받은 측에서는 그 암호를 해독하여 출력하는 처리가 필요하다.



### 통신 상대를 확인하지 않기 때문에 위장이 가능하다.

HTTP 에 의한 통신에는 상대가 누구인지 확인하는 처리는 없기 때문에 누구든지 리퀘스트를 보낼 수 있다. IP 주소나 포트 등에서 그 웹 서버에 액세스 제한이 없는 경우 리퀘스트가 오면 상대가 누구든지 무언가의 리스폰스를 반환한다. 이러한 특징은 여러 문제점을 유발한다.

1. 리퀘스트를 보낸 곳의 웹 서버가 원래 의도한 리스폰스를 보내야 하는 웹 서버인지를 확인할 수 없다.
2. 리스폰스를 반환한 곳의 클라이언트가 원래 의도한 리퀘스트를 보낸 클라이언트인지를 확인할 수 없다.
3. 통신하고 있는 상대가 접근이 허가된 상대인지를 확인할 수 없다.
4. 어디에서 누가 리퀘스트 했는지 확인할 수 없다.
5. 의미없는 리퀘스트도 수신한다. —> DoS 공격을 방지할 수 없다.

#### 보완 방법

위 암호화 방법으로 언급된 `SSL`로 상대를 확인할 수 있다. SSL 은 상대를 확인하는 수단으로 **증명서** 를 제공하고 있다. 증명서는 신뢰할 수 있는 **제 3 자 기관에 의해** 발행되는 것이기 때문에 서버나 클라이언트가 실재하는 사실을 증명한다. 이 증명서를 이용함으로써 통신 상대가 내가 통신하고자 하는 서버임을 나타내고 이용자는 개인 정보 누설 등의 위험성이 줄어들게 된다. 한 가지 이점을 더 꼽자면 클라이언트는 이 증명서로 본인 확인을 하고 웹 사이트 인증에서도 이용할 수 있다.



### 완전성을 증명할 수 없기 때문에 변조가 가능하다

여기서 완전성이란 **정보의 정확성** 을 의미한다. 서버 또는 클라이언트에서 수신한 내용이 송신측에서 보낸 내용과 일치한다라는 것을 보장할 수 없는 것이다. 리퀘스트나 리스폰스가 발신된 후에 상대가 수신하는 사이에 누군가에 의해 변조되더라도 이 사실을 알 수 없다. 이와 같이 공격자가 도중에 리퀘스트나 리스폰스를 빼앗아 변조하는 공격을 중간자 공격(Man-in-the-Middle)이라고 부른다.

#### 보완 방법

`MD5`, `SHA-1` 등의 해시 값을 확인하는 방법과 파일의 디지털 서명을 확인하는 방법이 존재하지만 확실히 확인할 수 있는 것은 아니다. 확실히 방지하기에는 `HTTPS`를 사용해야 한다. SSL 에는 인증이나 암호화, 그리고 다이제스트 기능을 제공하고 있다.



### HTTPS

> HTTP 에 암호화와 인증, 그리고 완전성 보호를 더한 HTTPS

`HTTPS`는 SSL 의 껍질을 덮어쓴 HTTP 라고 할 수 있다. 즉, HTTPS 는 새로운 애플리케이션 계층의 프로토콜이 아니라는 것이다. HTTP 통신하는 소켓 부분을 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 프로토콜로 대체하는 것 뿐이다. HTTP 는 원래 TCP 와 직접 통신했지만, HTTPS 에서 HTTP 는 SSL 과 통신하고 **SSL 이 TCP 와 통신** 하게 된다. SSL 을 사용한 HTTPS 는 암호화와 증명서, 안전성 보호를 이용할 수 있게 된다.

HTTPS 의 SSL 에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 사용한다. 공통키를 공개키 암호화 방식으로 교환한 다음에 다음부터의 통신은 공통키 암호를 사용하는 방식이다.

#### 모든 웹 페이지에서 HTTPS를 사용해도 될까?

평문 통신에 비해서 암호화 통신은 CPU나 메모리 등 리소스를 더 많이 요구한다. 통신할 때마다 암호화를 하면 추가적인 리소스를 소비하기 때문에 서버 한 대당 처리할 수 있는 리퀘스트의 수가 상대적으로 줄어들게 된다.

하지만 최근에는 하드웨어의 발달로 인해 HTTPS를 사용하더라도 속도 저하가 거의 일어나지 않으며, 새로운 표준인 HTTP 2.0을 함께 이용한다면 오히려 HTTPS가 HTTP보다 더 빠르게 동작한다. 따라서 웹은 과거의 민감한 정보를 다룰 때만 HTTPS에 의한 암호화 통신을 사용하는 방식에서 현재 모든 웹 페이지에서 HTTPS를 적용하는 방향으로 바뀌어가고 있다.

#### Reference

- https://tech.ssut.me/https-is-faster-than-http/

<br/>

클라이언트-서버 모델을 따르는 프로토콜로 TCP/IP 위에서 동작하며 well-known 포트인 80번 포트를 사용하여 통신한다. 첫번째 표준은 HTTP/1.1이며 이후로 HTTP/2 및 HTTP/3가 등장하였다. 여기선 HTTP/1.1의 내용을 정리한다.

## 특징

### 비-연결 지향 (Connectionless)

클라이언트가 서버에게 리소스를 요청한 후 응답을 받으면 연결을 끊어버리는 특징이다. 연결을 유지하게 되면 서버에 많은 부담을 줄 수 있기 때문에 상당히 많은 클라이언트에게 요청을 받는 웹 서버의 경우 응답을 처리했으면 연결을 끊는다. 이로 인해 서버의 부담을 줄일 수 있지만, 리소스를 요청할 때마다 연결해야 하는 오버헤드 비용이 발생한다. 이를 해결하기 위해선, 요청 헤더의 `Connection: keep-alive` 속성으로 지속적 연결 상태(Persistent connection)를 유지할 수 있다. 즉, 요청을 할 때마다 연결하지 않고 기존의 연결을 재사용하는 방식이다. HTTP 1.1 부턴 지속적 연결 상태가 기본이며 이를 해제하기 위해선 명시적으로 요청 헤더를 수정해야 한다.

### 무상태성 (Stateless)

각각의 요청이 독립적으로 여겨지는 특징으로, 서버는 클라이언트의 상태를 유지하지 않는다. 즉, 각 클라이언트에 맞게 리소스를 응답하는 것은 불가능하다. 이를 해결하기 위해, 쿠키나 세션 또는 토큰 방식의 OAuth 및 JWT가 사용된다.



## Method

클라이언트가 서버에 요청방법을 정의하는 것으로 주어진 리소스에 수행하길 원하는 행동을 나타낸다.

- **GET** : 서버에게 조회할 리소스를 요청한다. (READ, 조회)
- **POST** : 서버에게 본문(body)에 생성할 데이터를 삽입하여 전송한다. (CREATE, 생성)
- **PUT** : 서버에게 본문에 수정할 데이터를 삽입하여 전송한다. (UPDATE, 수정)
- **DELETE** : 서버에게 삭제할 리소스를 요청한다. (DELETE, 삭제)
- **PATCH** : PUT과 비슷하지만 일부만 수정한다는 점에서 다르다.



## 응답 상태코드

서버가 클라이언트에게 요청을 받으면 응답상태에 따라서 다른 상태코드를 클라이언트에게 돌려준다.

- **1xx (요청에 대한 정보)** : 요청을 받았으면 작업을 계속한다.

- 2xx (성공)

   

  : 요청을 성공적으로 수행했다.

  - 200(성공), 201(새 리소스 작성), 202(요청 접수, 아직 처리는 안함)

- 3xx (리다이렉션)

   

  : 클라이언트가 요청을 마지기 위해 추가적인 동작을 취해야 한다.

  - 300(여러개의 응답, 선택해야 함), 301(영구이동, 요청한 페이지가 영구적으로 이동됨), 302(임시이동, 현재 응답잉 다른 페이지이긴 하지만 임시적임)

- 4xx (클라이언트 오류)

   

  : 클라이언트에 오류가 있다.

  - 401(권한 없음), 403(금지됨, 리소스에 대한 권한 없음), 404(찾을 수 없음, 서버에 없는 페이지)

- 5xx (서버 오류)

   

  : 서버에 오류가 있다.

  - 500(내부 서버오류), 501(요청수행 기능없음, 메서드 인식불가), 503(서비스 사용불가)



## 헤더

요청/응답 헤더 및 본문 헤더 등 다양한 속성들이 있지만 여기선 주요한 속성들만 명시한다.

### 요청 헤더

- Host : 서버의 도메인 이름과 TCP 포트번호 (표준 포트는 생략 가능)

  - ```
    Host: en.wikipedia.org:8080
    ```

- Content-Type : POST/PUT 메서드를 사용할 때 본문의 타입

  - ```
    Content-Type: application/x-www-form-urlencoded
    ```

- If-Modified-Since : 명시한 날짜 이후로 변경된 리소스만 획득

  - ```
    If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
    ```

- Origin : 요청이 어느 도메인에서 왔는지 명시, 서버의 `Access-Control-*` 속성에 필요

  - ```
    Origin: http://www.example-social-network.com
    ```

- Cookie : 서버의 `Set-Cookie` 로 설정된 쿠키 값

  - ```
    Cookie: $Version=1; Skin=new;
    ```

### 응답 헤더

- Access-Control-* : CORS를 허용하기 위한 웹사이트 명시

  - ```
    Access-Control-Allow-Origin: *
    ```

- Set-Cookie : 클라이언트에 쿠키 설정

  - ```
    Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1
    ```

- Last-Modified : 요청한 리소스가 마지막으로 변경된 시각

  - ```
    Last-Modified: Tue, 15 Nov 1994 12:45:26 GMT
    ```

- Location : 3xx 상태 코드일 때, 리다이렉션 되는 주소

  - ```
    Location: http://www.w3.org/pub/WWW/People.html
    ```

- Allow : 요청한 리소스에 대해 가능한 메서드들

  - ```
    Allow: GET, HEAD
    ```



## 참고

- [MDN, HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
- [위키백과, HTTP 상태 코드](https://ko.wikipedia.org/wiki/HTTP_상태_코드)
- [Wikipedia, List of HTTP header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- [🙈[HTTP\] HTTP 특성(비연결성, 무상태)과 구성요소 그리고 Restful API🐵](https://victorydntmd.tistory.com/286)
- [[Network\] HTTP 헤더의 종류 및 항목](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)



<br/>

# HTTPS

[![img](https://github.com/baeharam/Must-Know-About-Frontend/raw/main/images/network/https.png)](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/images/network/https.png)

HTTPS(HyperText Transfer Protocol over TLS/SSL)는 **기존의 HTTP를 암호화한 프로토콜** 로 보안이 강화된 버전이다. 약어에서의 "S"가 원래 SSL(Secure Socket Layer)의 약자였지만 SSL 버전 3.1부터 TLS(Transport Layer Security)로 명칭이 바뀌고 TLS와 혼용하고 있다. TCP의 연결이 이루어진 후 TLS를 통해 암호화 설정이 되고 통신을 하는 방식이다.



## 필요한 개념

- **공개키(Public Key)와 비밀키(Private Key)** : 공개키는 모두가 볼 수 있는 키이며 비밀키는 소유자만이 가지고 있는 키로 암/복호화에 사용된다.
- **대칭키 암호화** : 서버와 클라이언트가 암호화/복호화에 동일한 비밀키를 사용하는 방식, 키를 공유하는데 어려움이 있으나 속도가 빠르다.
- **비대칭키 암호화** : 서버와 클라이언트가 암호화/복호화에 각각 다른 비밀키를 사용하는 방식, 공개키를 통해서 암호화를 하고 비밀키를 통해서 복호화를 한다. 공개키는 공개해도 상관없으니 키 관리에 어려움이 없으나, 속도가 느리다.
- **인증기관(Certificate Authority, CA)** : 클라이언트가 접속을 요청한 서버가 의도한 서버가 맞는지 인증해주는 역할을 하는 보증된 기업들이다. 클라이언트는 서버에 요청을 해서 CA가 발급한 인증서를 받은 뒤 CA의 공개키로 복호화하여 신뢰할 만한 인증서인지 검증한다. CA의 공개키로 복호화되는 암호화는 오직 CA의 비밀키로 암호화한 경우밖에 없기 때문에 복호화되면 신뢰할 만한 것이다.



## 동작방식

HTTPS는 **대칭키 암호화를 사용** 하며 다음과 같은 과정을 거친다.

1. 클라이언트가 서버에게 접속요청을 하면 서버는 CA에서 발급받은 인증서를 보낸다. 인증서에는 CA의 비밀키로 암호화된 사이트정보와 공개키가 들어있다.
2. 클라이언트는 인증서를 받아 CA의 공개키로 복호화하여 접속요청한 서버가 신뢰할만한지 검증한다.
3. 복호화가 되면 인증서가 신뢰할 만하기 때문에 데이터를 주고받을 대칭키를 생성한다.
4. 대칭키를 서버의 공개키로 암호화하여 서버에게 전송한다.
5. 서버는 자신의 비밀키로 클라이언트가 보낸 대칭키를 복호화한 뒤 그 대칭키를 통해 데이터를 주고받는다.

더 상세하게 들어가면 암호화 알고리즘, 난수 등 좀 더 복잡하지만 이런 과정을 알면 충분하다고 생각한다. [SSL.com의 영상](https://vimeo.com/135666049) 이 정말 알기쉽게 설명했기 때문에 정리하면서 이걸 보도록 하자.



## 참고

- [SSL 동작 방식을 간단히 알아보기](https://offbyone.tistory.com/274)
- [HTTPS와 SSL인증서](https://opentutorials.org/course/228/4894)



<br/>



- #####  HTTP(HyperText Transfer Protocol)

  인터넷 상에서 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약



HTTP는 텍스트 교환이므로, 누군가 네트워크에서 신호를 가로채면 내용이 노출되는 보안 이슈가 존재한다.

이런 보안 문제를 해결해주는 프로토콜이 **'HTTPS'**



- ##### HTTPS(HyperText Transfer Protocol Secure)

  인터넷 상에서 정보를 암호화하는 SSL 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약

HTTPS는 텍스트를 암호화한다. (공개키 암호화 방식으로!) : [공개키 설명](https://github.com/kim6394/tech-interview-for-developer/blob/master/Computer Science/Network/대칭키 %26 공개키.md)





#### HTTPS 통신 흐름

1. 애플리케이션 서버(A)를 만드는 기업은 HTTPS를 적용하기 위해 공개키와 개인키를 만든다.
2. 신뢰할 수 있는 CA 기업을 선택하고, 그 기업에게 내 공개키 관리를 부탁하며 계약을 한다.

***CA란?*** : Certificate Authority로, 공개키를 저장해주는 신뢰성이 검증된 민간기업

1. 계약 완료된 CA 기업은 해당 기업의 이름, A서버 공개키, 공개키 암호화 방법을 담은 인증서를 만들고, 해당 인증서를 CA 기업의 개인키로 암호화해서 A서버에게 제공한다.
2. A서버는 암호화된 인증서를 갖게 되었다. 이제 A서버는 A서버의 공개키로 암호화된 HTTPS 요청이 아닌 요청이 오면, 이 암호화된 인증서를 클라이언트에게 건내준다.
3. 클라이언트는 `main.html` 파일을 달라고 A서버에 요청했다고 가정하자. HTTPS 요청이 아니기 때문에 CA기업이 A서버의 정보를 CA 기업의 개인키로 암호화한 인증서를 받게 된다.

CA 기업의 공개키는 브라우저가 이미 알고있다. (세계적으로 신뢰할 수 있는 기업으로 등록되어 있기 때문에, 브라우저가 인증서를 탐색하여 해독이 가능한 것)

1. 브라우저는 해독한 뒤 A서버의 공개키를 얻게 되었다. 이제 A서버와 통신할 대는 얻은 A서버의 공개키로 암호화해서 요청을 날리게 된다.



HTTPS도 무조건 안전한 것은 아니다. (신뢰받는 CA 기업이 아닌 자체 인증서 발급한 경우 등)

이때는 HTTPS지만 브라우저에서 `주의 요함`, `안전하지 않은 사이트`와 같은 알림으로 주의 받게 된다.



##### [참고사항]

[링크](https://jeong-pro.tistory.com/89)



<br/>







<br/>

## HTTP 1.1 ~ 2.0

> fsdfdf



# HTTP/1.1과 HTTP/2의 차이점

이제까지 전통적인 웹 브라우저와 웹 서버와의 통신 프로토콜은 HTTP/1.1 기준으로 동작했는데, 많은 문제점들과 개선할 부분들이 있었기 때문에 2015년에 HTTP/2가 등장하게 되었다.



## HTTP/1.1의 문제점

### HOLB(Head Of Line Blocking)

HTTP 요청을 할 때는 요청을 하고 나서 응답이 와야 다음 요청을 할 수 있었는데 HTTP/1.1에 들어오면서 파이프라이닝(Pipelining) 기법을 통해 응답을 받지 않고도 여러개의 요청을 연속적으로 할 수 있게 되었다. 하지만 이 또한 처음의 요청에 대한 응답이 오래 걸리는 경우, 그 다음 응답까지의 시간이 지연되는 현상이 발생한다. 이렇게 파이프라이닝 기법은 심각한 문제를 안고 있었으며 이를 Head of Line Blocking 문제라고 부른다.

### 무겁고 중복 많은 헤더 구조

요청을 할 때 요청헤더에 메타정보를 넣어서 보내게 되는데, 매 요청마다 보내는 정보가 많아져서 헤더가 무거워지고 쿠키 같은 경우는 계속 보내게 되기 때문에 중복도 많아지는 문제가 있다.



## HTTP/2가 나오기 전의 개선방법들

- CSS/JavaScript/이미지 압축
- Data URI 스키마(이미지를 이진 파일로 바꿔 HTML에 넣어놓는 기법)
- Image Sprite(여러개의 작은 이미지들을 1번의 요청으로 받아오는 기법)
- 도메인 샤딩(1개의 도메인을 여러개의 서브도메인으로 나눠서 병렬요청하는 기법)
- 스크립트 파일을 `</body>` 직전에 배치해서 HTML/CSS 파싱 중단 안되게 설정



## HTTP/2의 개선방법들

### 멀티플렉싱(Multiplexing)과 스트리밍(Streaming)

HTTP 요청 데이터는 헤더와 본문으로 구성되는데 이를 각각 프레임(Frame)이라는 단위로 지정하고 스트림(Stream)이라는 연결단위를 통해 헤더 프레임 혹은 본문 프레임을 보내도록 그 방식을 바꿨다. 하나의 스트림은 요청/응답으로 구성되고 여러개의 스트림을 생성할 수 있다. 바로 이것이 스트리밍을 통한 멀티플렉싱이다. 이를 통해 기존의 HTTP/1.1의 문제점인 HLOB를 해결할 수 있게 된다. 또한 요청한 리소스간의 우선순위를 설정하기 때문에 스트림 별로 가중치가 매겨지고 브라우저가 리소스들을 수신하는 순서를 적절하게 결정한다.

### 서버 푸시(Server Push)

브라우저가 요청하지 않으면 서버는 응답하지 않는 것이 보통이지만, 요청한 HTML 문서에 리소스가 포함되어 있는 경우 서버가 브라우저에게 밀어주는(push) 방식을 취하여 브라우저의 요청을 최소화시킨다.

### 헤더 압축(Header Compression)

헤더 테이블(Header Table)을 사용하여 이전 헤더 정보를 유지하고 허프만 인코딩 기법으로 헤더를 압축해서 전송하여 중복과 크기를 줄인다.



## 참고

- [MDN, HTTP/1.x의 커넥션 관리](https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x)
- [Google, HTTP/2 소개](https://developers.google.com/web/fundamentals/performance/http2)







## HTTP/2

- HTTP/2는 [HTTP/1.1](https://goodgid.github.io/HTTP-1.1/)에서 설명한것 처럼 [SPDY](https://goodgid.github.io/HTTP-1.1/#http11-단점-극복-결과)를 기반으로 2012년 10월 부터 시작한 새로운 프로토콜 구현 프로젝트 이다.
- http2 공식 github 페이지의 서문을 보면 http2의 목적을 명확히 알 수 있다.

```
HTTP/2 is a replacement for how HTTP is expressed “on the wire.” 
It is not a ground-up rewrite of the protocol

HTTP methods, status codes and semantics are the same, and it should be possible to use the same APIs as HTTP/1.x (possibly with some small additions) to represent the protocol. 

The focus of the protocol is on performance; specifically, end-user perceived latency, network and server resource usage. 

One major goal is to allow the use of a single connection from browsers to a Web site."
```

[번역](https://translate.google.co.kr/?#en/ko/HTTP%2F2 is a replacement for how HTTP is expressed “on the wire.”  It is not a ground-up rewrite of the protocol HTTP methods%2C status codes and semantics are the same%2C and it should be possible to use the same APIs as HTTP%2F1.x (possibly with some small additions) to represent the protocol.  The focus of the protocol is on performance%3B specifically%2C end-user perceived latency%2C network and server resource usage.  One major goal is to allow the use of a single connection from browsers to a Web site.")

- 즉 완전히 새로운 프로토콜을 만들었기 보단 성능 향상에 초점을 맞춘 프로토콜이다.

------

## HTTP/2의 성능 향상을 위한 요소

### Multiplexed Streams

- 한 커넥션으로 **동시에 여러개의 메세지**를 주고 받을 있으며 응답은 **순서**에 상관없이 stream으로 주고 받는다.
- HTTP/1.1의 **Connection [Keep-Alive](https://goodgid.github.io/HTTP-Keep-Alivemd)**, **Pipelining**의 개선이라 보면 된다.

![img](https://goodgid.github.io/assets/img/network/http_2_0_2.png)

------

### Stream Prioritization

- 예를 들면 클라이언트가 요청한 HTML문서안에 CSS파일 1개와 Image파일 2개가 존재할 때
- 이를 클라이언트가 각각 요청하고 난 후 Image파일보다 CSS파일의 수신이 늦어지는 경우
  브라우저의 렌더링이 늦어지는 문제가 발생하는데
  HTTP/2의 경우 **리소스간 의존관계(우선순위)**를 설정하여 이런 문제를 해결하고 있다.

![img](https://goodgid.github.io/assets/img/network/http_2_0_3.png)

------

### Server Push

- 서버는 클라이언트의 요청에 대해 요청하지도 않은 리소스를 마음대로 보내줄 수 도 있다.
- What? 클라이언트(브라우저)가 HTML문서를 요청했고
  해당 HTML에 여러개의 리소스(CSS, Image…) 가 포함되어 있는 경우
- HTTP/1.1에서 클라이언트는 요청한 HTML문서를 수신한 후
  HTML문서를 해석하면서 필요한 리소스를 재 요청하는 반면
- HTTP/2에선 Server Push 기법을 통해서
  클라이언트가 요청하지도 않은 (HTML문서에 포함된 리소스) 리소스를 Push 해주는 방법으로
  **클라이언트의 요청**을 **최소화**해서 **성능 향상**을 이끌어 낸다.
- 이를 **PUSH_PROMISE**라고 부르며
  PUSH_PROMISE를 통해서 서버가 전송한 리소스에 대해선 클라이언트는 요청을 하지 않는다.

![img](https://goodgid.github.io/assets/img/network/http_2_0_4.png)

------

### Header Compression

- HTTP/2는 Header 정보를 압축하기 위해 **Header Table**과 **Huffman Encoding**기법을 사용하여 처리하는데
- 이를 **HPACK 압축 방식**이라 부르며 별도의 [명세서(RFC 7531)](https://http2.github.io/http2-spec/compression.html)로 관리하고 있다.

![img](https://goodgid.github.io/assets/img/network/http_2_0_5.png)

- 위 그림처럼 클라이언트가 두번의 요청을 보낸다고 가정하면
  HTTP/1.x의 경우 두개의 요청 Header에 중복값이 존재해도 그냥 중복 전송한다.
- 하지만 HTTP/2에선 Header에 중복값이 존재하는 경우
  **Static/Dynamic Header Table 개념**을 사용하여
  **중복 Header를 검출**하고
  중복된 Header는 index값만 전송하고
  중복되지 않은 Header정보의 값은 Huffman Encoding 기법으로 인코딩처리 하여 전송한다.

![img](https://goodgid.github.io/assets/img/network/http_2_0_6.png)

------

## HTTP/1.1 과 HTTP/2 성능비교

- 두 프로토콜의 객관적인 성능비교 지표는
  테스트 환경과 각각 테스트시 외부 인터넷 품질등의 영향으로 정확하게 알 수는 없지만,
- 일반적으로 HTTP/2를 사용할 경우 웹 응답 속도가 HTTP/1.1에 비해 15~50%가 향상 된다고 한다.
- [성능 테스트 사이트](https://www.httpvshttps.com/)에서 동일 개수/용량의 png이미지를 웹사이트에 로딩시켜 HTTP/1.1 과 HTTP/2의 속도를 비교한 결과이다.

![img](https://goodgid.github.io/assets/img/network/http_2_0_7.png)

![img](https://goodgid.github.io/assets/img/network/http_2_0_8.png)

- HTTP/1.1은 HTTP/2에 비해 *594%* 나 느림을 알 수 있다.
- 이미지에 HTTPS라고 적혀있지만 실제로는 HTTP/2를 뜻한다.

------

## Reference

- [HTTP 동작 과정](http://jess-m.tistory.com/17)
- [나만 모르고 있던 - HTTP/2](https://www.popit.kr/나만-모르고-있던-http2/)

<br/>



## HTTP vs Socket

> fsdf

# HTTP 통신 vs Socket 통신

- 통신을 하는데 있어서 다음과 같이 크게 두 가지로 나눌 수 있다.

1. HTTP 통신
2. Socekt 통신

- HTTP와 Socket의 가장 큰 차이점은 **접속(Connection)**을 유지하는지의 여부이다.

------

## HTTP 통신

HTTP 통신은 웹브라우저에 정보를 표시하는 것과 같이 클라이언트의 요청이 있을 때

서버가 해당 페이지에 대한 자료를 전송하고 곧바로 연결을 끊는 방식이다.

현재 이 글을 보고 있는 상황에 맨 처음 이 페이지를 로드 시에만 서버와 연결이 되고

현재는 서버와 접속이 끊어진 상태이다.

이 상태에서 F5 키를 눌러 새로고침을 하거나 다른 페이지로 이동하면 그때 다시 서버와 연결을 한다.



이렇게 하는 이유는 단 한가지. 서버의 부하를 줄여서 다른 접속을 원활하게 처리하기 위해서이다.

만약 F5 키에 연필을 꽂아서 클라이언트가 서버를 계속해서 물고 늘어지면

서버는 이 클라이언트의 연결을 유지하느라 다른 컴퓨터의 응답이 늦어질 것이다.

이런 방식으로 여러 대의 PC가 서버를 붙잡고 늘어져서 서버가 다른 일을 하지 못하도록 하는 것을 **DDOS** 공격이라한다.

------

## Socket 통신

Socket 통신은 클라이언트가 서버와 접속이 되면 서버나 클라이언트에서 강제로 접속을 해제할 때까지는 계속해서 접속이 유지된다.

따라서 서버의 능력이 무한대가 아닌 이상 동시에 접속할 수 있는 클라이언트의 수가 제한이 될 수 밖에 없다.

Socket 통신은 실시간으로 정보 교환이 필요하는 채팅이나 온라인 게임, 실시간 동영상 강좌 등에 사용된다.

따라서 이와 같은 경우가 아니라면 서버와의 통신은 HTTP를 사용하는 것이 시스템의 자원을 보다 효과적으로 사용할 수 있다.

------

Back : [[로컬 영역 네트워크편\] Ethernet](https://goodgid.github.io/NW-Ethernet/)

Next : [[로컬 영역 네트워크편\] Token Ring](https://goodgid.github.io/NW-Toekn-Ring/)





<br/>

## 대칭키 & 공개키

> ㄹㄴㅇㄹ

#### 대칭키(Symmetric Key)

> 암호화와 복호화에 같은 암호키(대칭키)를 사용하는 알고리즘

동일한 키를 주고받기 때문에, 매우 빠르다는 장점이 있음

but, 대칭키 전달과정에서 해킹 위험에 노출



#### 공개키(Public Key)

> 암호화와 복호화에 사용하는 암호키를 분리한 알고리즘

자신이 가지고 있는 고유한 암호키(비밀키)로만 복호화할 수 있는 암호키(공개키)를 대중에 공개함



##### 공개키 암호화 방식 진행 과정

1. A가 웹 상에 공개된 'B의 공개키'를 이용해 평문을 암호화하여 B에게 보냄
2. B는 자신의 비밀키로 복호화한 평문을 확인, A의 공개키로 응답을 암호화하여 A에개 보냄
3. A는 자신의 비밀키로 암호화된 응답문을 복호화함

대칭키의 단점을 완벽하게 해결했지만, 암호화 복호화가 매우 복잡함

(암호화하는 키가 복호화하는 키가 서로 다르기 때문)





##### 대칭키와 공개키 암호화 방식을 적절히 혼합해보면?

> SSL 탄생의 시초가 됨

```
1. A가 B의 공개키로 암호화 통신에 사용할 대칭키를 암호화하고 B에게 보냄
2. B는 암호문을 받고, 자신의 비밀키로 복호화함
3. B는 A로부터 얻은 대칭키로 A에게 보낼 평문을 암호화하여 A에게 보냄
4. A는 자신의 대칭키로 암호문을 복호화함
5. 앞으로 이 대칭키로 암호화를 통신함
```

즉, 대칭키를 주고받을 때만 공개키 암호화 방식을 사용하고 이후에는 계속 대칭키 암호화 방식으로 통신하는 것!





<br/>



## Cookie - Session

> fsdf

# Cookie vs Session

HTTP는 상태가 없는(Stateless) 프로토콜이기 때문에 사용자가 웹 브라우저를 통해서 특정 웹 사이트에 접속하게 될 경우 어떤 사용자가 접속했는지에 대한 정보를 파악할 수 없다. 따라서, 쿠키 또는 세션을 사용하여 사용자를 구분하고 각 사용자에 맞는 정보를 제공한다.



## Cookie

쿠키란 클라이언트의 웹 브라우저에 저장되는 작은 데이터 조각으로 서버가 클라이언트의 요청을 식별하는데 사용된다. 쿠키를 활용해서 사용자를 구분하는게 매우 유용하지만, 클라이언트가 수정할 수도 있고 해커가 탈취할 수도 있기 때문에 보안에 취약하다. 따라서, 아이디 및 비밀번호와 같은 민감한 정보들을 저장하는데 사용하지는 않고 아래와 같은 목적으로 사용한다.

- **세션 ID 관리**, 서버에 저장해야 할 민감한 정보에 대한 식별자 ID
- **개인화**, 사용자 선호 및 테마
- **트래킹**, 사용자 행동 기록 및 분석



## Session

세션이란 브라우저가 서버에 연결되어 있는 동안 유지하는 데이터 집합이다. 사용자가 웹 사이트에 방문하여 서버에 요청을 보내게 되면, 사용자의 정보를 서버에 저장하고 그 정보를 식별할 수 있는 "세션 ID"를 `Set-Cookie` 헤더로 클라이언트에게 전송한다. 위에서 말했던 것처럼 클라이언트는 쿠키로 세션 ID를 관리하고 해당 서버에 요청할 때마다 `Cookie` 헤더에 세션 ID를 포함시켜 전송하기 때문에 서버는 클라이언트를 식별하여 그에 맞는 정보를 응답으로 줄 수 있게 된다. 따라서, 아래와 같은 목적으로 사용한다고 할 수 있다.

- **민감한 정보 관리**, 사용자의 비밀번호 및 개인정보



## 차이점

- 쿠키
  - 클라이언트 쪽에 저장한다. (웹 브라우저)
  - 브라우저가 꺼져도 삭제되지 않고 사용자가 삭제하거나 정해진 시간만큼 유지된다.
  - 문자열만 저장할 수 있다.
  - 클라이언트에서 보내기 때문에 속도가 빠르다.
  - 민감한 데이터를 스니핑 당할수도 있기 때문에 보안에 취약하다.
- 세션
  - 서버쪽에 저장한다. (서버의 메모리 혹은 데이터베이스)
  - 브라우저가 꺼질 경우 삭제된다.
  - 문자열 뿐 아니라 객체도 저장할 수 있다.
  - 서버쪽에서 처리하기 때문에 속도가 비교적 느리다.
  - 서버에서 민감한 데이터를 갖고 있기 때문에 비교적 보안이 좋다.



## 참고

- [MDN, HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)
- [쿠키,세션,캐시가 뭔가요? (유튜브)](https://www.youtube.com/watch?v=OpoVuwxGRDI)
- [What are sessions? How do they work?](https://stackoverflow.com/questions/3804209/what-are-sessions-how-do-they-work)
- [CS 142: Web Applications (Fall 2010)](https://web.stanford.edu/~ouster/cgi-bin/cs142-fall10/index.php)
- [Web - 쿠키와 세션의 차이, 용도, 사용법(cookie, session)](https://jeong-pro.tistory.com/80)
- [웹 서버 개발의 Session 전략](https://devhaks.github.io/2019/04/20/session-strategy/)



<br/>



## HTTP Keep Alive

> fsdff





<br/>

## 





## rest - api

> Fsdf

학술적으로 엄밀한 내용을 보고 싶다면 REST를 만든 [로이필딩의 논문](https://www.ics.uci.edu/~taylor/documents/2002-REST-TOIT.pdf) 을 보는 것이 좋다. 여기선 최대한 REST와 API가 무엇인지에 대해 체감하는 것을 목표로 한다.

## REST란 무엇인가?

REpresentational State Transfer의 약자로 전반적인 웹 어플리케이션에서 상호작용하는데 사용되는 웹 아키텍쳐 모델이다. 즉, 자원을 주고받는 **웹 상에서의 통신 체계에 있어서 범용적인 스타일을 규정한 아키텍쳐** 라고 할 수 있다.

## API란 무엇인가?

Application Programming Interface의 약자로 구글 맵 API, 카카오 비전 API 등 **기존에 있는 응용 프로그램을 통해서 데이터를 제공받거나 기능을 사용하고자 할 때 사용하는 인터페이스 및 규격** 을 말한다. API는 프로그래밍 언어, 운영체제 등에서도 사용되는 범용적인 용어이다. 따라서, REST API라는 것은 REST 원칙을 적용하여 서비스 API를 설계한 것을 말하며 대부분의 서비스가 REST API를 제공한다.



## REST의 특징들

### 균등한 인터페이스 (Uniform Interface)

REST가 HTTP의 표준만 따른다면 어떠한 기술이던지 접목하여 사용할 수 있기 때문에 플랫폼이나 언어의 제약에 구애받지 않는다. 요즘은 REST API를 정의할 때 JSON(JavaScript Object Notation) 방식을 가장 많이 사용하지만 XML(eXtensible Markup Language)도 적용할 수 있다.

### 무상태성 (Stateless)

서버는 클라이언트의 상황을 고려하지 않고 API 요청에 대해서만 처리하기 때문에 이를 **"상태가 없다"** 라고 표현한다. 이렇게 되면 클라이언트를 고려하지 않아도 되기 때문에 구현이 간결해진다.

### 캐싱 가능 (Cacheable)

REST는 HTTP 표준을 기반으로 만들어졌기 때문에 HTTP의 특징인 캐싱을 사용할 수 있다. REST API를 활용하여 `GET` 메소드를 `Last-Modified` 값과 함께 보낼 경우, 컨텐츠의 변화가 없을 때 캐시된 값을 사용하게 된다. 이렇게 되면 네트워크 응답시간 뿐만 아니라 API 서버에 요청을 발생시키지 않기 때문에 부담이 덜 하다는 장점 또한 가지게 된다.

### 자체 표현성 (Self-Descriptiveness)

REST API의 자원명시 규칙 및 메소드는 그 자체로 의미를 지니기 때문에 어떠한 요청에 있어서 그 요청 자체로 어떤 것을 표현하는지 알아보기 쉽다. 물론 API를 규정한 각 서비스들이 문서를 제공하지만 이 특성에 따라서 요청하는 방식만으로 어떠한 의미인지 알 수 있어야 좋은 REST API라고 할 수 있다.

### 클라이언트-서버 구조 (Client-Server Architecture)

REST 서버가 API를 제공하는 방식이기 때문에 클라이언트에서 처리하는 부분과 독립적으로 동작한다. 따라서, 서로간의 의존성이 줄어들고 클라이언트와 서버를 최대한 독립적으로 개발할 수 있도록 도와준다.

### 계층형 구조 (Layered System)

클라이언트는 계층형 구조가 불가능하지만 REST 서버의 경우, 보안/로드 밸런싱/암호화 등을 추가할 수 있고 Proxy 및 게이트웨이 등의 중간매체를 사용할 수 있다.



## REST API의 핵심

### URI는 리소스를 표현해야 한다.

- **리소스 명은 동사가 아닌 명사를 사용해야 한다.**

```
/students/1
```

- **리소스는 Collection과 Document로 표현할 수 있다.**

이 때 Collection은 **복수** 를 사용함을 주의하자.

```
/locations/seoul/schools/3
```

여기서 `locations` 는 Collection을, `seoul` 은 Document를 표현한다.



### 그 리소스에 대한 행위는 HTTP의 Method로 표현해야 한다.

- **GET은 리소스를 조회한다. (학생 목록 조회)**

```
GET /students
```

- **POST는 리소스를 생성한다. (학생 생성)**

```
POST /students
```

- **PUT은 리소스를 업데이트한다. (1번 학생 정보 업데이트)**

```
PUT /students/1
```

- **DELETE는 리소스를 삭제한다. (1번 학생 삭제)**

```
DELETE /students/1
```



## HTTP 상태코드

요청에 대한 응답의 상태코드 또한 명확하게 돌려주는 것이 잘 설계된 REST API이다.

- **2xx** : 성공 관련 (200 Ok, 201 Created)
- **3xx** : 리다이렉션 관련 (304 Not Modified)
- **4xx** : 클라이언트 에러 관련 (400 Bad Request, 401 Unauthorized)
- **5xx** : 서버 에러 관련 (500 Internal Server Error)



## 잘못된 REST 사용

- **GET/POST의 부적합한 사용** : 기존의 조회/생성의 기능이 아닌 다른 방식으로 사용하는 경우이다.
- **자체 표현적이지 않음** : REST의 특징 중 하나인 자체표현성에서 떨어지는 경우로 이해하기 어렵다.
- **HTTP 응답 코드 미사용** : 위에서 정리한 응답에 관한 상태코드를 명확하게 정의하지 않은 경우이다.
- 그 외에 리소스 표현 가이드 및 REST의 특징을 위반한 경우들을 주의하자.



마지막으로 아래 그림을 보고 정리해보자.

[![img](https://github.com/baeharam/Must-Know-About-Frontend/raw/main/images/network/REST.png)](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/images/network/REST.png)



## 참고

- [REST API concepts and examples(유튜브)](https://www.youtube.com/watch?v=7YcW25PHnAA)
- [REST API가 뭔가요?(유튜브)](https://www.youtube.com/watch?v=iOueE9AXDQQ)
- [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
- [REST API의 이해와 설계-#1 개념 소개](https://bcho.tistory.com/953)
- [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)
- [Why Should We Choose REST (Client-Server) Model to Develop Web Apps ?](https://medium.com/@audira98/why-should-we-choose-rest-client-server-model-to-develop-web-apps-c3bb2451b13a)





<br/>



<br/>

## TCP 3-way-handshake, 4-way-handshake

> fsdf





<br/>

## TCP vs UDP

> fsdf







<br/>



## DNS Round Robin 방식

> ㄹㄴㅇㄹ





<br/>

## 웹 통신의 큰 흐름

> ㄹㄴㅇㄹㅇㄹ



<br/>

## 흐름 제어 & 혼잡 제어 & 오류제어

> ㄹㄴㅇㄹ



<br/>

## OSI 7 계층

> ㄹㄴㅇㄹ





<br/>

## Blocking Non-Blocking IO

> fsdf









<br/>

## 로드 밸런싱 Load Balancing

> fsdf





<br/>



## CDN

> fsdf



<br/>



## type - url - process

> fsf







<br/>

## uri

> fsdf









<br/>

## CORS

> fsdf





<br/>

## OSI 참조 모델과 TCP/IP 기초 - IP 구조, IP, 노드, OSI 참조 모델, 패킷, 서브넷 마스크, TCP/IP

> fdsf



<br/>



## 로컬 영역 네트워크편 - Ethernet, Token Ring, 무선 LAN

> ㅇㄴㄹ





<br/>

## 와이드 영역 네트워크 - VPN

> fsdf







<br/>

## 하드웨어 - 브리지, 게이트웨이, 리피터, 라우터

> ㄹㅇㄴ





<br/>

## 서비스 프로토콜 - DHCP, DNS, NAT , 프록시 서버

> ㄹㄴㅇㄹ







<br/>

## 인터넷편 - ISP, Java, RSS , SOAP, URL

> F fsd



<br/>

## 네트워크의 주소에 대한 모든 것

> ㄹㄴㅇㄹ





<br/>

## 다양한 TImeout을 알아보자 - Connection Timeout, Socket Timeout, Read Timeout

> fsdf



<br/>

## HTTP를 사용한 통신 과정

> ㄹㄴㅇㄹ



<br/>

## Forward Proxy & Reverse Proxy

> fsdfds





<br/>

## 네이버 메인 페이지의 트래픽 처리

> ㄹㄴㅇㅇㄹ







<br/>

<br/>



<br/>

<br/>

<br/>

<br/>

<br/>

<br/>

<br/>

<br/><br/>

