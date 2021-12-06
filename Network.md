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

-----------

# HTTP method : GET vs POST

> 기능 공통점 차이점, HTTP Request Message, 데어터 크기, 보안, 브라우저에서 Caching 여부, 

둘 다 HTTP 프로토콜을 이용해서 서버에 무엇인가를 요청할 때 사용하는 방식이다. 하지만 둘의 특징을 제대로 이해하여 기술의 목적에 맞게 알맞은 용도에 사용해야한다.

### GET

우선 GET 방식은 요청하는 데이터가 `HTTP Request Message`의 Header 부분에 url 이 담겨서 전송된다. 때문에 url 상에 `?` 뒤에 데이터가 붙어 request 를 보내게 되는 것이다. 이러한 방식은 url 이라는 공간에 담겨가기 때문에 전송할 수 있는 데이터의 크기가 제한적이다. 또 보안이 필요한 데이터에 대해서는 데이터가 그대로 url 에 노출되므로 `GET`방식은 적절하지 않다. (ex. password)

### POST

POST 방식의 request 는 `HTTP Request Message`의 Body 부분에 데이터가 담겨서 전송된다. 때문에 바이너리 데이터를 요청하는 경우 POST 방식으로 보내야 하는 것처럼 데이터 크기가 GET 방식보다 크고 보안면에서 낫다.(하지만 보안적인 측면에서는 암호화를 하지 않는 이상 고만고만하다.)

*그렇다면 이러한 특성을 이해한 뒤에는 어디에 적용되는지를 알아봐야 그 차이를 극명하게 이해할 수 있다.* 우선 GET 은 가져오는 것이다. 서버에서 어떤 데이터를 가져와서 보여준다거나 하는 용도이지 서버의 값이나 상태 등을 변경하지 않는다. SELECT 적인 성향을 갖고 있다고 볼 수 있는 것이다. 반면에 POST 는 서버의 값이나 상태를 변경하기 위해서 또는 추가하기 위해서 사용된다.

부수적인 차이점을 좀 더 살펴보자면 GET 방식의 요청은 브라우저에서 Caching 할 수 있다. 때문에 POST 방식으로 요청해야 할 것을 보내는 데이터의 크기가 작고 보안적인 문제가 없다는 이유로 GET 방식으로 요청한다면 기존에 caching 되었던 데이터가 응답될 가능성이 존재한다. 때문에 목적에 맞는 기술을 사용해야 하는 것이다.

<br/>

## HTTP method

클라이언트가 서버에 요청방법을 정의하는 것으로 주어진 리소스에 수행하길 원하는 행동을 나타낸다.

- **GET** : 서버에게 조회할 리소스를 요청한다. (READ, 조회)
- **POST** : 서버에게 본문(body)에 생성할 데이터를 삽입하여 전송한다. (CREATE, 생성)
- **PUT** : 서버에게 본문에 수정할 데이터를 삽입하여 전송한다. (UPDATE, 수정)
- **DELETE** : 서버에게 삭제할 리소스를 요청한다. (DELETE, 삭제)
- **PATCH** : PUT과 비슷하지만 일부만 수정한다는 점에서 다르다.

<br/>

#### 응답 상태코드

서버가 클라이언트에게 요청을 받으면 응답상태에 따라서 다른 상태코드를 클라이언트에게 돌려준다.

- **1xx (요청에 대한 정보)** : 요청을 받았으면 작업을 계속한다.

- 2xx (성공) : 요청을 성공적으로 수행했다.

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

### 헤더

요청/응답 헤더 및 본문 헤더 등 다양한 속성들이 있지만 여기선 주요한 속성들만 명시한다.

#### 요청 헤더

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

#### 응답 헤더

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



<br/>

-------

<br/>

# HTTP vs HTTPS

> HTTP의 문제점 3가지와 보완 방법
>
> HTTP vs HTTPS 특징
>
> HTTPS 통신 소켓, HTTPS 동작 방식(공개키 암호화 방식) 및 통신 흐름

<br/>

## HTTP(HyperText Transfer Protocol)

인터넷 상에서 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약

HTTP는 텍스트 교환이므로, 누군가 네트워크에서 신호를 가로채면 내용이 노출되는 보안 이슈가 존재한다.

이런 보안 문제를 해결해주는 프로토콜이 **'HTTPS'**

<br/>

클라이언트-서버 모델을 따르는 프로토콜로 TCP/IP 위에서 동작하며 well-known 포트인 80번 포트를 사용하여 통신한다. 첫번째 표준은 HTTP/1.1이며 이후로 HTTP/2 및 HTTP/3가 등장하였다. 여기선 HTTP/1.1의 내용을 정리한다.

### 특징

- #### 비-연결 지향 (Connectionless)

클라이언트가 서버에게 리소스를 요청한 후 응답을 받으면 연결을 끊어버리는 특징이다. 연결을 유지하게 되면 서버에 많은 부담을 줄 수 있기 때문에 상당히 많은 클라이언트에게 요청을 받는 웹 서버의 경우 응답을 처리했으면 연결을 끊는다. 이로 인해 서버의 부담을 줄일 수 있지만, 리소스를 요청할 때마다 연결해야 하는 오버헤드 비용이 발생한다. 이를 해결하기 위해선, 요청 헤더의 `Connection: keep-alive` 속성으로 지속적 연결 상태(Persistent connection)를 유지할 수 있다. 즉, 요청을 할 때마다 연결하지 않고 기존의 연결을 재사용하는 방식이다. HTTP 1.1 부턴 지속적 연결 상태가 기본이며 이를 해제하기 위해선 명시적으로 요청 헤더를 수정해야 한다.

- #### 무상태성 (Stateless)

각각의 요청이 독립적으로 여겨지는 특징으로, 서버는 클라이언트의 상태를 유지하지 않는다. 즉, 각 클라이언트에 맞게 리소스를 응답하는 것은 불가능하다. 이를 해결하기 위해, 쿠키나 세션 또는 토큰 방식의 OAuth 및 JWT가 사용된다.

<br/>

### HTTP 의 문제점 3가지

- HTTP 는 평문 통신이기 때문에 도청이 가능하다.
- 통신 상대를 확인하지 않기 때문에 위장이 가능하다.
- 완전성을 증명할 수 없기 때문에 변조가 가능하다.

*위 세 가지는 다른 암호화하지 않은 프로토콜에도 공통되는 문제점들이다.*

<br/>

### 문제점 1. TCP/IP 는 도청 가능한 네트워크이다.

TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있다. 패킷을 수집하는 것만으로 도청할 수 있다. 평문으로 통신을 할 경우 메시지의 의미를 파악할 수 있기 때문에 암호화하여 통신해야 한다.

- 보안 방법
  1. 통신 자체를 암호화 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 다른 프로토콜을 조합함으로써 HTTP 의 통신 내용을 암호화할 수 있다. SSL 을 조합한 HTTP 를 `HTTPS(HTTP Secure)` or `HTTP over SSL`이라고 부른다.
  2. 콘텐츠를 암호화 말 그대로 HTTP 를 사용해서 운반하는 내용인, HTTP 메시지에 포함되는 콘텐츠만 암호화하는 것이다. 암호화해서 전송하면 받은 측에서는 그 암호를 해독하여 출력하는 처리가 필요하다.

<br/>

### 문제점 2. 통신 상대를 확인하지 않기 때문에 위장이 가능하다.

HTTP 에 의한 통신에는 상대가 누구인지 확인하는 처리는 없기 때문에 누구든지 리퀘스트를 보낼 수 있다. IP 주소나 포트 등에서 그 웹 서버에 액세스 제한이 없는 경우 리퀘스트가 오면 상대가 누구든지 무언가의 리스폰스를 반환한다. 이러한 특징은 여러 문제점을 유발한다.

1. 리퀘스트를 보낸 곳의 웹 서버가 원래 의도한 리스폰스를 보내야 하는 웹 서버인지를 확인할 수 없다.
2. 리스폰스를 반환한 곳의 클라이언트가 원래 의도한 리퀘스트를 보낸 클라이언트인지를 확인할 수 없다.
3. 통신하고 있는 상대가 접근이 허가된 상대인지를 확인할 수 없다.
4. 어디에서 누가 리퀘스트 했는지 확인할 수 없다.
5. 의미없는 리퀘스트도 수신한다. —> DoS 공격을 방지할 수 없다.

- 보완 방법

   위 암호화 방법으로 언급된 `SSL`로 상대를 확인할 수 있다. SSL 은 상대를 확인하는 수단으로 **증명서** 를 제공하고 있다. 증명서는 신뢰할 수 있는 **제 3 자 기관에 의해** 발행되는 것이기 때문에 서버나 클라이언트가 실재하는 사실을 증명한다. 이 증명서를 이용함으로써 통신 상대가 내가 통신하고자 하는 서버임을 나타내고 이용자는 개인 정보 누설 등의 위험성이 줄어들게 된다. 한 가지 이점을 더 꼽자면 클라이언트는 이 증명서로 본인 확인을 하고 웹 사이트 인증에서도 이용할 수 있다.

<br/>

### 문제점 3. 완전성을 증명할 수 없기 때문에 변조가 가능하다

여기서 완전성이란 **정보의 정확성** 을 의미한다. 서버 또는 클라이언트에서 수신한 내용이 송신측에서 보낸 내용과 일치한다라는 것을 보장할 수 없는 것이다. 리퀘스트나 리스폰스가 발신된 후에 상대가 수신하는 사이에 누군가에 의해 변조되더라도 이 사실을 알 수 없다. 이와 같이 공격자가 도중에 리퀘스트나 리스폰스를 빼앗아 변조하는 공격을 중간자 공격(Man-in-the-Middle)이라고 부른다.

- 보완 방법

  `MD5`, `SHA-1` 등의 해시 값을 확인하는 방법과 파일의 디지털 서명을 확인하는 방법이 존재하지만 확실히 확인할 수 있는 것은 아니다. 확실히 방지하기에는 `HTTPS`를 사용해야 한다. SSL 에는 인증이나 암호화, 그리고 다이제스트 기능을 제공하고 있다.

<br/>

## HTTPS(HyperText Transfer Protocol Secure)

인터넷 상에서 정보를 암호화하는 SSL 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약

HTTPS는 텍스트를 암호화한다. (공개키 암호화 방식으로!) : [공개키 설명](https://github.com/kim6394/tech-interview-for-developer/blob/master/Computer Science/Network/대칭키 %26 공개키.md)

HTTP 에 암호화와 인증, 그리고 완전성 보호를 더한 HTTPS

`HTTPS`는 SSL 의 껍질을 덮어쓴 HTTP 라고 할 수 있다. 즉, HTTPS 는 새로운 애플리케이션 계층의 프로토콜이 아니라는 것이다. HTTP 통신하는 소켓 부분을 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 프로토콜로 대체하는 것 뿐이다. HTTP 는 원래 TCP 와 직접 통신했지만, HTTPS 에서 HTTP 는 SSL 과 통신하고 **SSL 이 TCP 와 통신** 하게 된다. SSL 을 사용한 HTTPS 는 암호화와 증명서, 안전성 보호를 이용할 수 있게 된다.

HTTPS 의 SSL 에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 사용한다. 공통키를 공개키 암호화 방식으로 교환한 다음에 다음부터의 통신은 공통키 암호를 사용하는 방식이다.

<br/>

[![img](https://github.com/baeharam/Must-Know-About-Frontend/raw/main/images/network/https.png)](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/images/network/https.png)

HTTPS(HyperText Transfer Protocol over TLS/SSL)는 **기존의 HTTP를 암호화한 프로토콜** 로 보안이 강화된 버전이다. 약어에서의 "S"가 원래 SSL(Secure Socket Layer)의 약자였지만 SSL 버전 3.1부터 TLS(Transport Layer Security)로 명칭이 바뀌고 TLS와 혼용하고 있다. TCP의 연결이 이루어진 후 TLS를 통해 암호화 설정이 되고 통신을 하는 방식이다.

<br/>

#### 모든 웹 페이지에서 HTTPS를 사용해도 될까?

평문 통신에 비해서 암호화 통신은 CPU나 메모리 등 리소스를 더 많이 요구한다. 통신할 때마다 암호화를 하면 추가적인 리소스를 소비하기 때문에 서버 한 대당 처리할 수 있는 리퀘스트의 수가 상대적으로 줄어들게 된다.

하지만 최근에는 하드웨어의 발달로 인해 HTTPS를 사용하더라도 속도 저하가 거의 일어나지 않으며, 새로운 표준인 HTTP 2.0을 함께 이용한다면 오히려 HTTPS가 HTTP보다 더 빠르게 동작한다. 따라서 웹은 과거의 민감한 정보를 다룰 때만 HTTPS에 의한 암호화 통신을 사용하는 방식에서 현재 모든 웹 페이지에서 HTTPS를 적용하는 방향으로 바뀌어가고 있다.

<br/>



## 필요한 개념

- **공개키(Public Key)와 비밀키(Private Key)** : 공개키는 모두가 볼 수 있는 키이며 비밀키는 소유자만이 가지고 있는 키로 암/복호화에 사용된다.
- **대칭키 암호화** : 서버와 클라이언트가 암호화/복호화에 동일한 비밀키를 사용하는 방식, 키를 공유하는데 어려움이 있으나 속도가 빠르다.
- **비대칭키 암호화** : 서버와 클라이언트가 암호화/복호화에 각각 다른 비밀키를 사용하는 방식, 공개키를 통해서 암호화를 하고 비밀키를 통해서 복호화를 한다. 공개키는 공개해도 상관없으니 키 관리에 어려움이 없으나, 속도가 느리다.
- **인증기관(Certificate Authority, CA)** : 클라이언트가 접속을 요청한 서버가 의도한 서버가 맞는지 인증해주는 역할을 하는 보증된 기업들이다. 클라이언트는 서버에 요청을 해서 CA가 발급한 인증서를 받은 뒤 CA의 공개키로 복호화하여 신뢰할 만한 인증서인지 검증한다. CA의 공개키로 복호화되는 암호화는 오직 CA의 비밀키로 암호화한 경우밖에 없기 때문에 복호화되면 신뢰할 만한 것이다.

<br/>

## 동작방식

HTTPS는 **대칭키 암호화를 사용** 하며 다음과 같은 과정을 거친다.

1. 클라이언트가 서버에게 접속요청을 하면 서버는 CA에서 발급받은 인증서를 보낸다. 인증서에는 CA의 비밀키로 암호화된 사이트정보와 공개키가 들어있다.
2. 클라이언트는 인증서를 받아 CA의 공개키로 복호화하여 접속요청한 서버가 신뢰할만한지 검증한다.
3. 복호화가 되면 인증서가 신뢰할 만하기 때문에 데이터를 주고받을 대칭키를 생성한다.
4. 대칭키를 서버의 공개키로 암호화하여 서버에게 전송한다.
5. 서버는 자신의 비밀키로 클라이언트가 보낸 대칭키를 복호화한 뒤 그 대칭키를 통해 데이터를 주고받는다.

더 상세하게 들어가면 암호화 알고리즘, 난수 등 좀 더 복잡하지만 이런 과정을 알면 충분하다고 생각한다. [SSL.com의 영상](https://vimeo.com/135666049) 이 정말 알기쉽게 설명했기 때문에 정리하면서 이걸 보도록 하자.

<br/>

## HTTPS 통신 흐름

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

<br/>

ref

- [SSL 동작 방식을 간단히 알아보기](https://offbyone.tistory.com/274)
- [HTTPS와 SSL인증서](https://opentutorials.org/course/228/4894)

- https://tech.ssut.me/https-is-faster-than-http/

- https://jeong-pro.tistory.com/89

<br/>

---------

<br/>

# HTTP 1.1 ~ 2.0

> 기본 정의, 특징 2가지, method, 응답 상태코드, 헤더(요청,응답), 
>
> HTTP 1.1 vs HTTP 2.0 차이점 & 성능 비교
>
> HTTP 1.1 문제점 2가지
>
> HTTP 2.0 이전과 이후의 개선안들
>
> HTTP 2.0 목적, 성능 향상을 위한 요소

<br/>

### HTTP/1.1과 HTTP/2의 차이점

이제까지 전통적인 웹 브라우저와 웹 서버와의 통신 프로토콜은 HTTP/1.1 기준으로 동작했는데, 많은 문제점들과 개선할 부분들이 있었기 때문에 2015년에 HTTP/2가 등장하게 되었다.

### HTTP/1.1의 문제점

- #### HOLB(Head Of Line Blocking)

HTTP 요청을 할 때는 요청을 하고 나서 응답이 와야 다음 요청을 할 수 있었는데 HTTP/1.1에 들어오면서 파이프라이닝(Pipelining) 기법을 통해 응답을 받지 않고도 여러개의 요청을 연속적으로 할 수 있게 되었다. 하지만 이 또한 처음의 요청에 대한 응답이 오래 걸리는 경우, 그 다음 응답까지의 시간이 지연되는 현상이 발생한다. 이렇게 파이프라이닝 기법은 심각한 문제를 안고 있었으며 이를 Head of Line Blocking 문제라고 부른다.

- #### 무겁고 중복 많은 헤더 구조

요청을 할 때 요청헤더에 메타정보를 넣어서 보내게 되는데, 매 요청마다 보내는 정보가 많아져서 헤더가 무거워지고 쿠키 같은 경우는 계속 보내게 되기 때문에 중복도 많아지는 문제가 있다.

<br/>

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

<br/>

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

- [MDN, HTTP/1.x의 커넥션 관리](https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x)
- [Google, HTTP/2 소개](https://developers.google.com/web/fundamentals/performance/http2)

- [MDN, HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
- [위키백과, HTTP 상태 코드](https://ko.wikipedia.org/wiki/HTTP_상태_코드)
- [Wikipedia, List of HTTP header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- [🙈[HTTP\] HTTP 특성(비연결성, 무상태)과 구성요소 그리고 Restful API🐵](https://victorydntmd.tistory.com/286)
- [[Network\] HTTP 헤더의 종류 및 항목](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)

- [HTTP 동작 과정](http://jess-m.tistory.com/17)
- [나만 모르고 있던 - HTTP/2](https://www.popit.kr/나만-모르고-있던-http2/)

<br/>

--------

<br/>

# HTTP vs Socket

> HTTP vs Socket

## HTTP 통신 vs Socket 통신

- 통신을 하는데 있어서 다음과 같이 크게 두 가지로 나눌 수 있다.

1. HTTP 통신
2. Socekt 통신

- HTTP와 Socket의 가장 큰 차이점은 **접속(Connection)**을 유지하는지의 여부이다.



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



<br/>

-----

<br/>

# 대칭키 & 공개키

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

# Cookie - Session

> HTTP 특성 연관, 

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

# REST API

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

-------

-------------



<br/>

# TCP 3-way-handshake, 4-way-handshake

> 연결을 성립하고 해제하는 과정을 말한다

<br/>

## 3 way handshake - 연결 성립

TCP/IP 프로토콜을 이용하여 통신을 진행할 때, 두 종단 간 **정확한** 데이터 전송을 보장하기 위해 연결을 설정하는 과정이다.

TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기에 앞서, 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행한다.

[![img](https://camo.githubusercontent.com/4acea6af95884347810f057d00c6c4643a56d4a7dbbdf49740745560cd45cc1f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f5443502d636f6e6e656374696f6e2d312e706e67)](https://camo.githubusercontent.com/4acea6af95884347810f057d00c6c4643a56d4a7dbbdf49740745560cd45cc1f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f5443502d636f6e6e656374696f6e2d312e706e67)

1. 클라이언트가 서버에게 SYN 패킷을 보냄 (sequence : x)
2. 서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (sequence : y, ACK : x + 1)
3. 클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄

<br/>

이렇게 3번의 통신이 완료되면 연결이 성립된다. (3번이라 3 way handshake인 것)

<br/>

TCP가 호스트 간에 연결을 설정하는 방법으로 SYN/ACK 패킷을 통해 이루어진다. SYN 패킷은 동기화(SYNchronize)를 의미하는 패킷이며 ACK 패킷은 확인(ACKnowledgement)을 의미하는 패킷이다.

[![img](https://github.com/baeharam/Must-Know-About-Frontend/raw/main/images/network/3-way-handshake.png)](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/images/network/3-way-handshake.png)

- **LISTEN** : 서버가 클라이언트의 연결요청을 기다리고 있다.
- **SYN_SENT** : 클라이언트가 능동적으로 서버에게 연결요청을 하자고 시퀀스 번호를 생성하여 SYN 패킷에 담아 보낸다. (능동 개방)
- **SYN_RECEIVED** : SYN 패킷을 받은 서버는 자신만의 시퀀스 번호를 생성하여 SYN 패킷에 담고 클라이언트의 SYN 패킷에 있는 시퀀스 번호에 1을 더해서 ACK 패킷에 담아 같이 보낸다.
- **클라이언트 ESTABLISHED** : SYN+ACK 패킷을 받은 클라이언트는 ACK 패킷의 시퀀스 번호를 보고 자신이 보낸 시퀀스 번호와 차이가 1임을 확인한다. 차이가 1이라면 제대로 연결되었다고 판단하고 서버의 SYN 패킷에 있는 시퀀스 번호에 1을 더해 ACK 패킷에 담아 보낸다.
- **서버 ESTABLISHED** : 클라이언트의 ACK 패킷을 받고 그 안의 시퀀스 번호가 보냈던 SYN 패킷의 시퀀스 번호 + 1이라면 연결이 되었다고 판단한다. 이후부터 본격적인 통신을 할 수 있게 된다.

<br/>

- **TCP 3 Way Handshake**는 **전송 계층**의 **TCP/IP 프로토콜**을 이용해서 통신을 하는 응용프로그램이
  데이터를 전송하기 전에 **정확한 전송**을 보장하기 위해 상대방 컴퓨터와 사전에 **세션**을 **수립**하는 과정을 의미한다.

- 한마디로 **신뢰성**을 보장하기위해 사용한다.

- 확인, 응답을 뜻하는

   

  ACK

  는 ‘Acknowledgment’의 약자이다.

  - 신뢰적 데이터 전송을 위해 사용된다.

- 동기화 요청을 뜻하는

   

  SYN

  은 ‘Synchronize sequence numbers’의 약자이다.

  - 이 때 SYN 패킷의 Sequence Number는 난수를 이용해 생성한다.
  - 처음 클라이언트에서 SYN 패킷을 보낼 때 Sequence Number에는 랜덤한 숫자가 담겨진다.
  - 초기 **Sequence Number**를 **ISN**이라고 한다.
  - ISN이 0부터 시작하지 않고 난수를 생성해서 number를 설정하는 이유는 무엇일까?
  - Connection을 맺을 때 사용하는 포트(port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다.
  - 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다.
  - 서버 측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순차적인 number가 전송된다면 이전의 connection으로부터 오는 패킷으로 인식할 수 있다.
  - 이러한 **문제**가 **발생할 가능성**을 줄이기 위해서 **난수**로 **ISN**을 설정하는 것이다.

------

## 3-way Handshaking 역할

- 3-Way Handshaking 세션을 **수립**하기 위해 수행되는 절차이다.
- TCP는 장치들 사이에 논리적인 접속을 성립(Establish)하기 위하여 3-way Handshaking 사용한다.
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고
  실제로 데이터 전달이 시작하기전에 한쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
- 양쪽 모두 상대편에 대한 초기 **순차 일련 변호**를 얻을 수 있도록 한다.

![img](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_1.png)

------

## 3-way Handshaking 과정

```
Client > Server : TCP SYN
Server > Client : TCP SYN ACK
Client > Server : TCP ACK
```

- [STEP 1]
  - A 클라이언트는 B 서버에 접속을 요청하는 **SYN 패킷**을 보낸다.
  - 이때 A 클라이언트는 **SYN**을 보내고 **SYN/ACK** 응답을 기다리는 **SYN_SENT** 상태가 된다.
- [STEP 2]
  - B 서버는 **SYN**요청을 받고 A 클라이언트에게 요청을 수락한다는 **ACK**
    클라이언트도 포트를 열어달라는 **SYN**가 설정된 **패킷**을 발송하고 A가 다시 **ACK**으로 응답하기를 기다린다.
  - 이때 B 서버는 **SYN_RECEIVED** 상태가 된다.
- [STEP 3]
  - A 클라이언트는 B 서버에게 **ACK**을 보내고 이후부터는 연결이 이루어지고 데이터를 주고 받을 수 있게 된다.
  - 이때의 B 서버 상태가 **ESTABLISHED**이다.
- 위 같은 방식으로 통신하는 것이 **신뢰성**있는 연결을 맺어 준다는 **TCP의 3 Way handshake**방식이다.



<br/>

**[Q. 왜 2way가 아니라 3way일까?]**

**TCP**는 `양방향성 연결`이기 때문에 클라이언트에서 서버에게 자신의 존재를 알리고 패킷을 보낼 수 있는 것처럼 서버에서도 클라이언트에게 자신의 존재를 알리고 패킷을 보낼 수 있다는 신호를 보내야 하기 때문이다.

**[참고]**

**처음에 클라이언트에서 SYN 패킷을 보낼 때 Sequence Number에는 랜덤한 숫자가 담겨진다.**

Connection을 맺을 때, 사용하는 포트(port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용한다. 따라서 이전에 사용한 포트 번호를 재사용할 가능성이 있다.

Sequence Number가 순차적인 숫자로 전송된다면 서버는 이전의 Connection으로부터 전송되는 패킷으로 인식할 수 있다. 따라서 이러한 문제를 해결하기 위해 난수로 초기 Sequence Number를 설정한다.

<br/>

## 4 way handshake - 연결 해제

연결 성립 후, 모든 통신이 끝났다면 해제해야 한다.

[![img](https://camo.githubusercontent.com/8bb8960e46a3bfada6a237a7a91bce75a0a3e0e34eab5c1f5143ca6fe34d0b5f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f434e2e706e67)](https://camo.githubusercontent.com/8bb8960e46a3bfada6a237a7a91bce75a0a3e0e34eab5c1f5143ca6fe34d0b5f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f434e2e706e67)

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.
2. 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다. (이때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다)
3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.
4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. (아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다.)

- 서버는 ACK를 받은 이후 소켓을 닫는다 (Closed)
- TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)



이렇게 4번의 통신이 완료되면 연결이 해제된다.

<br/>

- 4-Way Handshaking 세션을 **종료**하기 위해 수행되는 절차이다.

![img](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_2.png)

------

## 4-way Handshaking 과정

![img](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_4.png)

- 최초에는 서로 통신 상태이기 때문에 양쪽이 ESTABLISHED 상태이다.
- 한 가지 주의할 점은 일반적으로 클라이언트가 FIN을 보내고 서버가 그 패킷을 받는 그림으로 4 way Handshaking을 학습하는데
- 무조건 클라이언트만 FIN 패킷을 보내는 것은 아니다.
- 따라서 클라이언트와 서버가 아닌 **Active Close(또는 Initiator, 기존 클라이언트)**와 **Passive Close(또는 Receiver, 기존 서버)**로 표현하는 것이 정확하다.
- 서버가 먼저 종료하겠다고 FIN을 보낼 수 있고 이런 경우 서버가 FIN_WAIT1 상태가 된다.
- [STEP 1]
  - 통신을 종료하고자 클라이언트가 서버에게 **FIN 패킷**을 보내고 자신은 **FIN_WAIT_1** 상태로 대기한다.
- [STEP 2]
  - **FIN 패킷**을 받은 서버는 해당 포트를 **CLOSE_WAIT** 상태로 바꾸고
  - 서버는 일단 **ACK**를 클라이언트에게 보내고
  - ACK를 받은 클라이언트는 상태를 **FIN_WAIT_2** 상태로 변경한다.
  - 그와 동시에 서버에서는 해당 포트에 연결되어 있는 **Application**에게 **Close()**를 요청한다.
- [STEP 3]
  - Close() 요청을 받은 Application은 종료 프로세스를 진행시켜 최종적으로 Close()가 되고
  - 서버는 **FIN 패킷**을 클라이언트에게 전송하고 상태를 **LAST_ACK** 상태로 바꾼다.
- [STEP 4]
  - 클라이언트는 **FIN_WAIT_2** 상태에서 서버가 연결을 종료했다는 신호를 기다리다가
  - **FIN**을 받으면 확인 했다는 의미로 **ACK**를 서버에게 전송한다.
  - 그리고 클라이언트는 **TIME_WAIT** 상태로 바꾼다.
  - **TIME_WAIT** 상태에서 일정 시간이 지나면 CLOSED 되게 된다. (이 상태는 아래 질문의 답이 된다.)
  - **최종 ACK**를 받은 서버는 자신의 포트도 CLOSED로 닫게 된다.



> Q. [ 필요 패킷 -> FIN 패킷 ] 순서로 클라이언트에게 전달이 이뤄져야하는데
> [ FIN 패킷 -> 필요 패킷 ] 순서로 전달이 이뤄졌을 경우 어떻게 해야할까?

- 서버에서 FIN 패킷을 전송하기 전에
  전송한 패킷이 **Routing 지연** or **패킷 유실로 인한 재전송** 등으로
  FIN 패킷보다 늦게 도착하는 상황이 발생할 수 있다.
- 클라이언트에서 세션을 종료시킨 후 뒤늦게 도착하는 패킷이 있다면
  이 패킷은 **Drop**되고 **데이터**는 **유실**될 것이다.
- 이러한 현상에 대비하여 클라이언트는 서버로부터 **FIN 패킷**을 수신하더라도
  일정시간(디폴트 240초)동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거치게 되는데
  이 과정을 **TIME_WAIT**라고 한다.

> Q. [비정상 종료 상황] 다양한 상황에 따른 연결의 종료를 적절하게 처리하지 못하여 CLOSE_WAIT 상태로 남아있는 상황

- Application에서 close()를 적절하게 처리해주지 못하면
  TCP 포트는 CLOSE_WAIT 상태로 계속 기다리게 된다.
- 이렇게 CLOSE_WAIT 상태가 statement에 많아지게 되면
  Hang이 걸려 더이상 연결을 하지 못하는 경우가 생기기도 한다.
- 따라서 어플리케이션 개발시 여러 상황에 따라 close() 처리를 잘 해줘야 한다.

> Q. [비정상 종료 상황] 다양한 상황에 따른 연결의 종료를 적절하게 처리하지 못하여 FIN_WAIT_1 상태로 남아있는 상황

- FIN_WAIT_1 상태라는 것은 상대방측에 커넥션 종료 요청을 했는데
  ACK를 받지 못한 상태로 기다리고 있는 것이다.
- 이것은 아마 서버를 찾을 수 없는 것으로 **네트워크 및 방화벽**의 문제일 수 있다.
- FIN_WAIT_1의 상태는 일정 시간이 지나면 **Time Out**이 되면 스스로 Closed하게 된다.

> Q. [비정상 종료 상황] 다양한 상황에 따른 연결의 종료를 적절하게 처리하지 못하여 FIN_WAIT_2 상태로 남아있는 상황

- FIN_WAIT_2 상태는 클라이언트가 서버에 종료를 요청한 후
  서버에서 요청을 접수했다고 ACK를 받았지만
  서버에서 종료를 완료했다는 FIN 을 받지 못하고 기다리고 있는 상태이다.
- 이 상태는 양방의 두번의 통신이 이루어졌기 때문에 **네트워크의 문제**는 **아닌 것**으로 판단된다.
- 서버측에서 CLOSE를 처리하지 못하는 경우일 수도 있다.
- FIN_WAIT_2 역시 일정 시간이 지나면 **Time Out**이 되면 스스로 Closed하게 된다.



- 어떠한 이유에서 FIN_WAIT_1과 FIN_WAIT_2 상태인 연결이 많이 남아있다면 문제가 발생할 수 있다.
- 물론 일정 시간이 지나 Time Out이 되면 연결이 자동으로 종료되긴 하지만
  이 Time Out이 길어져 많은 수의 소켓이 늘어나게 되면 **메모리 부족**으로 더 이상 소켓을 오픈하지 못하는 경우가 발생한다.
- 이 경우는 **네트워크나 방화벽** 또는 **Application**에서 close()처리 등에 대한 문제등으로 발생할 수 있으며 원인을 찾기가 쉽지 않다.
- 이러한 문제 해결을 위해서 FIN_WAIT_1과 FIN_WAIT_2 의 **Time Out** 시간을 적절히 조절할 필요가 있다.

<br/>

[에러 발생]

클라이언트에서 FIN 패킷 전송 후 ACK 패킷을 기다리는 FIN_WAIT1과 서버의 ACK 패킷을 받은 후 FIN 패킷을 기다리는 FIN_WAIT2 에러 발생으로 인해 Time out이 되면 스스로 연결을 종료한다.

그러나, CLOSE_WAIT은 Application이 close()를 적절하게 처리하지 못하면 CLOSE_WAIT 상태로 계속 기다리게 되어 Socket Hang Up 에러가 발생할 수 있다.

<br/>



#### Reference

- http://asfirstalways.tistory.com/356
- [링크](https://www.geeksforgeeks.org/tcp-connection-termination/)
- [TCP 3-way Handshake, 4-way Handshake](http://lelumiere.tistory.com/10?category=670812)
- [TCP 연결 종료와 비정상 종료 4 way handshake (FIN_WAIT_1, FIN_WAIT_2, CLOSE_WAIT, LAST_ACK, TIME_WAIT](http://hyeonstorage.tistory.com/287)
- [TCP의 3 way Handshake과 4 way Handshake](http://needjarvis.tistory.com/157)
- [CLOSE_WAIT & TIME_WAIT 최종 분석](http://tech.kakao.com/2016/04/21/closewait-timewait/)
- [[TCP\] 3-way-handshake & 4-way-handshake](http://asfirstalways.tistory.com/356)
- [네트워크 :: 네트워크?](https://woovictory.github.io/2018/05/17/network/)

<br/>

-------------

-------



<br/>

# TCP vs UDP

> fsdf

OSI 7계층에서 전송계층(Transport layer)에 속하는 데이터 전송 프로토콜이다. 여기선 중요한 것만 살펴보고 깊게는 들어가지 않는다.

<br/>

## UDP

`UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)`는 **비연결형 프로토콜** 이다. IP 데이터그램을 캡슐화하여 보내는 방법과 연결 설정을 하지 않고 보내는 방법을 제공한다. `UDP`는 흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 **하지 않는다.** 이 모두가 사용자 프로세스의 몫이다. `UDP`가 행하는 것은 포트들을 사용하여 IP 프로토콜에 인터페이스를 제공하는 것이다.

종종 클라이언트는 서버로 짧은 요청을 보내고, 짧은 응답을 기대한다. 만약 요청 또는 응답이 손실된다면, 클라이언트는 time out 되고 다시 시도할 수 있으면 된다. 코드가 간단할 뿐만 아니라 TCP 처럼 초기설정(initial setup)에서 요구되는 프로토콜보다 적은 메시지가 요구된다.

`UDP`를 사용한 것들에는 `DNS`가 있다. 어떤 호스트 네임의 IP 주소를 찾을 필요가 있는 프로그램은, DNS 서버로 호스트 네임을 포함한 UDP 패킷을 보낸다. 이 서버는 호스트의 IP 주소를 포함한 UDP 패킷으로 응답한다. 사전에 설정이 필요하지 않으며 그 후에 해제가 필요하지 않다.

<br/>

TCP와 달리 데이터의 신뢰성을 보장하지 않는 프로토콜이며 다음 특징들을 갖는다.

- **비연결형(Connection-less)** 으로 연결을 설정하고 해제하는 과정이 없다.

- 신뢰성이 없고

   

  전송되는

   

  데이터의 순서를 보장하지 않는다.

  - 흐름제어, 혼잡제어가 없다.
  - 에러감지는 헤더의 체크섬(Checksum)을 이용한 정도밖에 없다.

- 패킷의 단위가 **데이터그램(Datagram)** 으로 **경계가 분명** 하여 수신자는 송신자가 보낸 그대로의 크기로 받게 된다.

- 서버와 클라이언트는 유니캐스트(1:1), 브로드캐스트(1:N), 멀티캐스트(1:M)가 가능하다. (N은 전체, M은 일부)

- TCP에 비해서 하는 작업들이 굉장히 적기 때문에 **속도가 빠르다.**

- DNS, DHCP, 비디오/오디오 스트리밍 등에 사용된다.

<br/>

- User Datagram Protocol의 약자이다.
- 데이터를 데이터 그램 단위로 처리하는 프로토콜.
  - 데이터 그램 : 독립적인 관계를 지니는 패킷
- 비연결형 프로토콜로 사전에 연결 설정 없이 데이터를 전달한다.
- 사전에 연결 설정을 하지 않은 데이터 그램 방식을 통해 데이터를 전달하기 때문에 하나의 메시지에서 분할된 각각의 패킷은 서로 다른 경로로 전송될 수 있다.
- 송신측에서 전송한 패킷의 순서와 수신측에 도착한 패킷의 순서가 다를 수 있다. 그러나 서로 다른 경로로 패킷을 처리함에도 불구하고 순서를 부여하거나 재조립하지 않는다.
- 흐름 제어, 혼잡 제어, 오류 제어를 하지 않으므로 손상된 세그먼트에 대한 재전송을 하지 않는다.
- 이로 인해 속도가 빠르며 네트워크 부하가 적다는 장점이 있지만, 신뢰성 있는 데이터 전송을 보장하지 못한다.
- UDP는 RTP(Real Time Protocol), Multicast, DNS 등에서 사용된다

DNS 같은 경우 누군가 DNS 서비스를 요청할 때마다 TCP처럼 Session을 맺고 통신한다면 속도도 느리고, 서버 리소스도 엄청나게 소모될 것이다.

그런가 하면 NMS(Network Management Server)가 5분에 한번씩 장비 상태를 점검하기 위해 정보를 읽어오는데 수백, 수천대의 장비와 Session을 맺어야 한다면 이것도 마찬가지로 문제가 생긴다.

그렇기 때문에 UDP를 사용한다.

재전송을 하면 안되는 서비스가 있다.

대표적으로 RTP이다.

전화를 하고 있다 다고 가정해보자.

"여","보","세","요"라는 4개의 데이터를 전송했는데, "세"를 못받았다고 다시 보내달라고 하면 "여보요세"가 될 것이다.

이럴 때는 그냥 "여보X요"로 전달하는게 나은 상황이다.

또한, Multicast 서비스가 UDP를 사용한다.

1:N으로 통신하는 방식에서 한 사람이 데이터를 받지 못했다고 재전송을 요청한다고 가정해보자. 제대로 받은 사람들도 해당 데이터를 다시 받아서 처리해야 한다는 문제점이 발생할 수 있기 때문에 UDP를 사용한다.

<br/>

#### 들어가기 전

- UDP 통신이란?

  - User Datagram Protocol의 약자로 데이터를 데이터그램 단위로 처리하는 프로토콜이다.
  - 비연결형, 신뢰성 없는 전송 프로토콜이다.
  - 데이터그램 단위로 쪼개면서 전송을 해야하기 때문에 전송 계층이다.
  - Transport layer에서 사용하는 프로토콜.

- TCP와 UDP는 왜 나오게 됐는가?

  1. IP의 역할은 Host to Host (장치 to 장치)만을 지원한다. 장치에서 장치로 이동은 IP로 해결되지만, 하나의 장비안에서 수많은 프로그램들이 통신을 할 경우에는 IP만으로는 한계가 있다.
  2. 또한, IP에서 오류가 발생한다면 ICMP에서 알려준다. 하지만 ICMP는 알려주기만 할 뿐 대처를 못하기 때문에 IP보다 위에서 처리를 해줘야 한다.

  - 1번을 해결하기 위하여 포트 번호가 나오게 됐고, 2번을 해결하기 위해 상위 프로토콜인 TCP와 UDP가 나오게 되었다.

  - *ICMP : 인터넷 제어 메시지 프로토콜로 네트워크 컴퓨터 위에서 돌아가는 운영체제에서 오류 메시지를 전송받는데 주로 쓰임

- 그렇다면 TCP와 UDP가 어떻게 오류를 해결하는가?

  - TCP : 데이터의 분실, 중복, 순서가 뒤바뀜 등을 자동으로 보정해줘서 송수신 데이터의 정확한 전달을 할 수 있도록 해준다.
  - UDP : IP가 제공하는 정도의 수준만을 제공하는 간단한 IP 상위 계층의 프로토콜이다. TCP와는 다르게 에러가 날 수도 있고, 재전송이나 순서가 뒤바뀔 수도 있어서 이 경우, 어플리케이션에서 처리하는 번거로움이 존재한다.

- UDP는 왜 사용할까?

  - UDP의 결정적인 장점은 데이터의 신속성이다. 데이터의 처리가 TCP보다 빠르다.
  - 주로 실시간 방송과 온라인 게임에서 사용된다. 네트워크 환경이 안 좋을때, 끊기는 현상을 생각하면 된다.

- DNS(Domain Name Service)에서 UDP를 사용하는 이유

  - Request의 양이 작음 -> UDP Request에 담길 수 있다.
  - 3 way handshaking으로 연결을 유지할 필요가 없다. (오버헤드 발생)
  - Request에 대한 손실은 Application Layer에서 제어가 가능하다.
  - DNS : port 53번
  - But, TCP를 사용할 때가 있다! 크기가 512(UDP 제한)이 넘을 때, TCP를 사용해야한다.



#### 1. UDP Header

- [![img](https://camo.githubusercontent.com/97937857395944ea8fa05caa629426bed8f78fbfd22c72d5a397b5344bc8609a/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323732413541333835373539323637423336)](https://camo.githubusercontent.com/97937857395944ea8fa05caa629426bed8f78fbfd22c72d5a397b5344bc8609a/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323732413541333835373539323637423336) - Source port : 시작 포트 - Destination port : 도착지 포트 - Length : 길이 - _Checksum_ : 오류 검출 - 중복 검사의 한 형태로, 오류 정정을 통해 공간이나 시간 속에서 송신된 자료의 무결성을 보호하는 단순한 방법이다.



- 이렇게 간단하므로, TCP 보다 용량이 가볍고 송신 속도가 빠르게 작동됨.
- 그러나 확인 응답을 못하므로, TCP보다 신뢰도가 떨어짐.
- UDP는 비연결성, TCP는 연결성으로 정의할 수 있음.



#### DNS과 UDP 통신 프로토콜을 사용함.

DNS는 데이터를 교환하는 경우임

이때, TCP를 사용하게 되면, 데이터를 송신할 때까지 세션 확립을 위한 처리를 하고, 송신한 데이터가 수신되었는지 점검하는 과정이 필요하므로, Protocol overhead가 UDP에 비해서 큼.

------

DNS는 Application layer protocol임.

모든 Application layer protocol은 TCP, UDP 중 하나의 Transport layer protocol을 사용해야 함.

(TCP는 reliable, UDP는 not reliable임) / DNS는 reliable해야할 것 같은데 왜 UDP를 사용할까?

사용하는 이유

1. TCP가 3-way handshake를 사용하는 반면, UDP는 connection 을 유지할 필요가 없음.

2. DNS request는 UDP segment에 꼭 들어갈 정도로 작음.

   DNS query는 single UDP request와 server로부터의 single UDP reply로 구성되어 있음.

3. UDP는 not reliable이나, reliability는 application layer에 추가될 수 있음. (Timeout 추가나, resend 작업을 통해)

DNS는 UDP를 53번 port에서 사용함.

------

그러나 TCP를 사용하는 경우가 있음.

Zone transfer 을 사용해야하는 경우에는 TCP를 사용해야 함.

(Zone Transfer : DNS 서버 간의 요청을 주고 받을 떄 사용하는 transfer)

만약에 데이터가 512 bytes를 넘거나, 응답을 못받은 경우 TCP로 함.

<br/>

> 데이터를 데이터그램 단위로 처리하는 프로토콜

- UDP : User Datagram Protocol
- 여기서 **데이터그램**이란 **독립적인 관계**를 지니는 **패킷**이라는 뜻으로
  UDP의 동작방식을 설명하자면 다음과 같다.
- 위에서 대충 눈치채셨듯이 TCP와 달리 UDP는 **비연결형 프로토콜**아다.
- 즉, 연결을 위해 할당되는 **논리적인 경로**가 없다.
- 그렇기 때문에 각각의 패킷은 다른 경로로 전송되고
  각각의 패킷은 독립적인 관계를 지니게 된다.

![img](https://goodgid.github.io/assets/img/network/tcp_udp_3.png)

- UDP는 TCP와는 달리
  메시지를 **패킷(데이터그램)**으로 나누고
  반대편에서 **재조립**하는 등의 서비스는 제공하지 않는다.
- **혼잡 제어**를 하지 않기 때문에 TCP보다 빠르다는 장점이 있다.
- UDP는 RTP(Real-Time Protocol), Multicast, 멀티미디어, VoIP, DNS의 [이름 해결](https://goodgid.github.io/Server-DNS/#dns-서버는-2종류)에서 사용된다.

```
DNS 같은 경우 누군가 DNS 서비스를 요청할 때마다 
TCP처럼 Session을 맺고 통신을 한다면 속도도 느리고, 서버 리소스도 엄청나게 소모될 것이다. 
그런가 하면 NMS(Network Management Server)가 5분에 한번씩 장비 상태를 점검하기 위해 정보를 읽어오는데 
수백, 수천대의 장비와 Session을 맺어야 한다면 이것도 마찬가지로 문제가 생긴다.
그렇기 때문에 UDP를 사용한다.
재전송을 하면 안되는 서비스들이 있다. 
대표적인 예가 바로 Real-Time Protocol이다. 
전화를 하고 있다고 가정을 해보자.
'여''보''세''요'라는 4개의 데이터를 전송했는데, 
'세'를 못 받았다고 다시 보내달라고 하면 
'여보요세'가 될 것이다. 
이럴 땐 그냥 '여보X요'로 전달하는게 더 나은 상황이다.

또한 Multicast 서비스가 UDP를 사용한다. 
1:N으로 통신하는 방식에서 
한놈이 데이터를 받지 못했다고 재전송 요청을 한다고 가정해보자. 
제대로 받은 놈들도 해당 데이터를 다시 받아서 처리해야 한다는 문제가 발생할 수 있다.
그렇기 때문에 UDP를 사용한다.
```

------

## TCP와 UDP 비교

![img](https://goodgid.github.io/assets/img/network/tcp_udp_4.png)

<br/>

## TCP

> 인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜

- TCP : Transmission Control Protocol
- 일반적으로 TCP와 IP를 함께 사용하는데, **IP**가 **데이터의 배달**을 **처리**한다면 **TC**P는 **패킷**을 **추적 및 관리**하게 된다.
- TCP는 **신뢰성**있는 데이터 전송을 지원하는 **연결지향형** 프로토콜이다.
- 연결지향형인 TCP는 **[3-way handshaking](https://goodgid.github.io/TCP-IP-3Way-4Way/#tcp-3-way-handshake)**이라는 과정을 통해 연결 후 통신을 시작한다.
- TCP에서 사용하는 포트번호의 수는 0 ~ 65535(=2^16)이다.
- 즉 총 65536개가 사용가능하다.
- 또한 포트 번호 범위는 의미를 갖는다.

```
## 잘 알려진 포트 / 등록 된 포트 / 동적 포트

0번 ~ 1023번: 잘 알려진 포트 (well-known port)
1024번 ~ 49151번: 등록된 포트 (registered port)
49152번 ~ 65535번: 동적 포트 (dynamic port)
```

- TCP 포트 0은 공식적으로 TCP/IP 네트워킹에서 예약 된 포트이다.
- 즉 TCP 또는 UDP 네트워크 통신에 사용해서는 안된다.
- 그러나 포트 0은 때때로 네트워크 프로그래밍, 특히 유닉스 소켓 프로그래밍에서 특별한 의미를 갖는다.
- 이 환경에서 포트 0은 시스템 할당 (동적) 포트를 지정하는 프로그래밍 기술이다.

```
TCP는 연결지향 프로토콜이라고 알려져 있는데, 
이것은 메시지들이 각단의 응용 프로그램들에 의해 교환되는 시간동안 연결이 확립되고 유지되는 것을 의미한다. 

TCP는 IP가 처리할 수 있도록 메시지를 여러 개의 패킷들로 확실히 나누고, 
반대편에서는 완전한 메시지로 패킷들을 재조립할 책임이 있다.
OSI 통신모델에서, TCP는 4계층인 트랜스포트 계층에 속한다.
```

- 인터넷 환경에서 기본으로 사용한다.

![img](https://goodgid.github.io/assets/img/network/tcp_udp_1.png)

- **흐름 제어**와 **혼잡 제어**를 지원하며 **데이터의 순서**를 **보장**한다.
- **[흐름 제어](https://goodgid.github.io/Error-Flow-Control/#흐름-제어)**란, 보내는 측과 받는 측의 데이터 처리속도 차이를 조절해주는 것을 말한다.
- **[혼잡 제어](https://goodgid.github.io/Error-Flow-Control/#혼잡-제어)**란, 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것을 말한다.
- TCP는 UDP에 비해 속도가 느리다는 단점이 있다.
- TCP는 대부분의 웹 HTTP 통신, 이메일, 파일전송에 사용된다.

![img](https://goodgid.github.io/assets/img/network/tcp_udp_2.png)

- TCP가 **가상 회선 방식**을 **제공**한다는 것은
  발신지와 수신지를 연결하여 패킷을 전송하기 위한 **논리적 경로**를 배정한다는 뜻이다.

> Q. 패킷(Packet)이란?

- 인터넷 내에서 데이터를 보내기 위한 **경로배정(라우팅)**을 효율적으로 하기 위해서
  **데이터**를 **여러 개의 조각들**로 나누어 전송을 하는데 이때, 이 조각을 **패킷**이라고 한다.

> Q. TCP는 패킷을 어떻게 추적 및 관리하는가?

- 데이터는 **패킷** 단위로 나누어 **같은 목적지(IP계층)**으로 전송된다.

```
예를 들어 한줄로 서야하는 A,B,C라는 사람(패킷)들이 서울(발신지)에서 출발하여 부산(수신지)으로 간다고 할 때
A,B,C가 순차적으로 가는 상황에서 B가 길을 잘못 들어서 분실되었다.
하지만 목적지에서는 A,B,C가 모두 필요한지 모르고 A,C만 보고 다 왔다고 착각할 수 있다. 
그렇기 때문에 A,,B,C라는 패킷에 1,2,3이라는 번호를 부여하여 
패킷의 분실 확인과 같은 처리를 하여 목적지에서 재조립을 한다. 
이런 방식으로 TCP는 패킷을 추적하며, 나누어 보내진 데이터를 받고 조립을 할 수 있게 된다.
예를 들면, HTML 파일이 웹 서버로부터 사용자에게 보내질 때, 
서버 내에 있는 TCP 프로그램 계층은 파일을 여러 개의 패킷들로 나누고, 패킷 번호를 붙인 다음, IP 프로그램 계층으로 보낸다. 

각 패킷이 동일한 수신지 주소(IP주소)를 가지고 있더라도, 
패킷들은 네트워크의 서로 다른 경로를 통해 전송될 수 있다. 
다른 한쪽 편(사용자 컴퓨터 내의 클라이언트 프로그램)에 있는 TCP는, 
각 패킷들을 재조립하고, 사용자에게 하나의 완전한 파일로 보낼 수 있을 때까지 기다린다.
```

------

## TCP / IP

- TCP/IP는 인터넷의 기본적인 통신 프로토콜로서 인트라넷이나 엑스트라넷과 같은 사설 망에서도 사용된다.
- 사용자가 인터넷에 접속하기 위해 자신의 컴퓨터를 설정할때 TCP/IP 프로그램이 설치되며
  이를 통하여 역시 같은 TCP/IP 프로토콜을 쓰고 있는 다른 컴퓨터 사용자와 메시지를 주고받거나 정보를 얻을 수 있게 된다.
- TCP/IP는 **2개의 계층**으로 이루어진 **프로그램**이다.
- 상위 계층인 **TCP**는 메시지나 파일들을 좀 더 **작은 패킷**으로 나누어 **인터넷**을 통해 **전송**하는 일과
  수신된 패킷들을 원래의 메시지로 **재조립**하는 일을 담당한다.
- 하위 계층인 **IP**는 **각 패킷의 주소 부분**을 처리함으로써
  패킷들이 목적지에 **정확하게 도달** 할 수 있게 한다.
- 네트워크 상의 각 게이트웨이는 메시지를 어느 곳으로 전달해야 할 지를 알기 위해 메시지의 주소를 확인한다.
- 한 메시지가 **여러 개의 패킷**으로 **나뉘어진 경우**
  각 패킷들은 서로 다른 경로를 통해 전달될 수 있으며
  그것들은 **최종 목적지**에서 **재조립**된다.
- TCP/IP는 통신하는데 있어 클라이언트/서버 모델을 사용하는데
  컴퓨터 사용자(클라이언트)의 요구에 대응하여 네트워크 상의 다른 컴퓨터(서버)가 웹 페이지를 보내는 식의 서비스를 제공한다.
- TCP/IP는 본래 **점대점(点對点)** 통신을 하는데
  이는 각 통신이 네트워크 상의 한 점(또는 호스트 컴퓨터)으로부터 시작되어 다른 점 또는 호스트 컴퓨터로 전달된다는 것을 의미한다.



<br/>



데이터가 반드시 전달되는 것을 보장하는 프로토콜로 다음 특징들을 갖는다.

- **연결지향(Connection-oriented)** 으로 2개의 호스트가 통신을 하기 전 연결이 이루어져야 한다.
- 높은 신뢰성(Reliability)  과 순서대로 전송하는 것(In-order delivery). 을 보장한다.
  - **흐름 제어(Flow control)** 를 통해 송신자의 데이터 양을 조절한다.
  - **혼잡 제어(Congestion control)** 를 통해 네트워크 상황을 감지하고 송신자의 데이터 양을 조절한다.
  - **에러 감지(Error detection)** 를 통해 잘못 전송되었을 경우 재전송한다.
- **전 이중(Full duplex) 방식** 으로 두 호스트 모두 송신자와 수신자가 될 수 있다.
- **바이트 스트림(Byte stream)** 을 사용하여 데이터를 연속적인 바이트로 보고, **세그먼트(Segment)** 라는 단위의 패킷으로 쪼개서 보낸다.
- HTTP, FTP, SMTP, TELNET 등에서 사용된다.



대부분의 인터넷 응용 분야들은 **신뢰성** 과 **순차적인 전달** 을 필요로 한다. UDP 로는 이를 만족시킬 수 없으므로 다른 프로토콜이 필요하여 탄생한 것이 `TCP`이다. `TCP(Transmission Control Protocol, 전송제어 프로토콜)`는 신뢰성이 없는 인터넷을 통해 종단간에 신뢰성 있는 **바이트 스트림을 전송** 하도록 특별히 설계되었다. TCP 서비스는 송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성함으로써 이루어진다. TCP 에서 연결 설정(connection establishment)는 `3-way handshake`를 통해 행해진다.

모든 TCP 연결은 전이중(full-duplex), 점대점(point to point)방식이다. 전이중이란 전송이 양방향으로 동시에 일어날 수 있음을 의미하며 점대점이란 각 연결이 정확히 2 개의 종단점을 가지고 있음을 의미한다. TCP 는 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.

<br/>

#### 들어가기 전

- TCP 통신이란?
  - 네트워크 통신에서 신뢰적인 연결방식
  - TCP는 기본적으로 unreliable network에서, reliable network를 보장할 수 있도록 하는 프로토콜
  - TCP는 network congestion avoidance algorithm을 사용
- reliable network를 보장한다는 것은 4가지 문제점 존재
  - 손실 : packet이 손실될 수 있는 문제
  - 순서 바뀜 : packet의 순서가 바뀌는 문제
  - Congestion : 네트워크가 혼잡한 문제
  - Overload : receiver가 overload 되는 문제
- 흐름제어/혼잡제어란?
  - 흐름제어 (endsystem 대 endsystem)
    - 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
    - Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것
    - 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점
  - 혼잡제어 : 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법
- 전송의 전체 과정
  - Application layer : sender application layer가 socket에 data를 씀.
  - Transport layer : data를 segment에 감싼다. 그리고 network layer에 넘겨줌.
  - 그러면 아랫단에서 어쨋든 receiving node로 전송이 됨. 이 때, sender의 send buffer에 data를 저장하고, receiver는 receive buffer에 data를 저장함.
  - application에서 준비가 되면 이 buffer에 있는 것을 읽기 시작함.
  - 따라서 flow control의 핵심은 이 receiver buffer가 넘치지 않게 하는 것임.
  - 따라서 receiver는 RWND(Receive WiNDow) : receive buffer의 남은 공간을 홍보함

<br/>

- 일반적으로 TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리한다.
- **신뢰성 있는 데이터 전송을 지원하는 연결 지향형 프로토콜이다.**
- 사전에 **3-way handshake**라는 과정을 통해 연결을 설정하고 통신을 시작한다.
- **4-way handshake** 과정을 통해 연결을 해제(가상 회선 방식)한다.
- **흐름 제어, 혼잡 제어, 오류 제어를 통해 신뢰성을 보장**한다. 그러나 이 때문에 UDP보다 전송 속도가 느리다는 단점이 있다.
- **데이터의 전송 순서를 보장**하며 수신 여부를 확인할 수 있다.
- TCP를 사용하는 예로는 대부분의 웹 HTTP 통신, 이메일, 파일 전송에 사용된다.
- TCP가 가상회선 방식을 제공한다는 것은 송신측과 수신측을 연결하여 패킷을 전송하기 위한 논리적 경로를 배정한다는 뜻이다.

> 패킷(Packet)?
>
> - 인터넷 내에서 데이터를 보내기 위한 경로 배정(라우팅)을 효율적으로 하기 위해서 데이터를 여러 개의 조각으로 나누어 전송을 하는데, 이때 조각을 패킷이라고 한다.

> TCP는 패킷을 어떻게 추적 및 관리하는가?
>
> - 데이터는 패킷 단위로 나누어 같은 목적지(IP 계층)으로 전송된다.

Ex) 한 줄로 서야 하는 A,B,C라는 사람(패킷)이 서울(송신측)에서 출발하여 부산(수신측)으로 가야한다고 가정하다.

A,B,C가 순차적으로 가는 상황에서 B가 길을 잘못 들어서 분실되었다.

하지만 목적지에서는 A,B,C가 모두 필요한지 모르고 A,C만 보고 다 왔다고 착각할 수 있다. 그렇기 때문에 A,B,C라는 패킷에 1,2,3이라는 **번호를 부여**하여 패킷의 분실 확인 처리를 하기 위해 목적지에서 패킷을 **재조립**한다.

이런 방식으로 TCP는 패킷을 추적하며, 나누어 보내진 데이터를 목적지에서 받고 재조립할 수 있게 된다.

## 흐름 제어

- 송신측과 수신측 사이의 데이터 처리 속도 차이(흐름)을 해결하기 위한 기법.
- 만약, 송신측의 전송량 > 수신측의 처리량일 경우 전송된 패킷은 수신측의 큐를 넘어서 손실될 수 있기 때문에 송신측의 패킷 전송량을 제어하게 된다.

1. `Stop and Wait(정지 - 대기)`

- **매번 전송한 패킷에 대한 확인 응답을 받아야 그 다음 패킷을 전송할 수 있다.**
- 이러한 구조 때문에 비효율적이다. (단점)
- Give & Take.

1. Sliding Window(슬라이딩 윈도우)

- (송신측 = 전송측)
- 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인 응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 기법이다.
- **윈도우** : 송신, 수신 스테이션 양쪽에서 만들어진 버퍼의 크기.
- Stop and Wait의 비효율성을 개선한 기법
- 송신측에서는 Ack 프레임을 수신하지 않더라도 여러 개의 프레임을 연속적으로 전송할 수 있다.
- 송신측에서 0,1,2,3,4,5,6을 보낼 수 있는 프레임을 가지고 있고 데이터 0,1을 전송했다고 가정하면 슬라이딩 윈도우의 구조는 2,3,4,5,6처럼 변하게 된다.
- 이때, 만약 수신측으로부터 ACK라는 프레임을 받게 된다면 송신측은 이전에 보낸 데이터 0,1을 수신측에서 정상적으로 받았음을 알게 되고 송신측의 슬라이딩 윈도우는 ACK 프레임에 따른 프레임의 수만큼 오른쪽으로 경계가 확장된다.

## 오류 제어

- 오류 검출과 재전송을 포함한다.
- ARQ(Automatic Repeat Request) 기법을 사용해 프레임이 손상되었거나 손실되었을 경우, 재전송을 통해 오류를 복구한다.
- ARQ 기법은 흐름 제어 기법과 관련되어 있다.

1. Stop and Wait ARQ

- 송신측에서 1개의 프레임을 송신하고, 수신측에서 수신된 프레임의 에러 유무 판단에 따라 ACK or NAK를 보내는 방식이다.
- 식별을 위해 데이터 프레임과 ACK 프레임은 각가 0,1 번호를 번갈아가며 부여한다.
- 수신측이 데이터를 받지 못했을 경우, NAK를 보내고 NAK를 받은 송신측은 데이터를 재전송한다.
- 만약, 데이터나 ACK가 분실되었을 경우 일정 간격의 시간을 두고 타임아웃이 되면, 송신측은 데이터를 재전송한다.

1. Go-Back-n ARQ(슬라이딩 윈도우)

- 전송된 프레임이 손상되거나 분실된 경우 그리고 ACK 패킷의 손실로 인한 TIME_OUT이 발생한 경우, 확인된 마지막 프레임 이후로 모든 프레임을 재전송한다.
- 슬라이딩 윈도우는 연속적인 프레임 전송 기법으로 전송측은 전송된 모든 프레임의 복사본을 가지고 있어야 하며, ACK와 NAK 모두 각각 구별해야 한다.
- ACK : 다음 프레임을 전송.
- NAK : 손상된 프레임 자체 번호를 반환.

**[재전송 되는 경우]**

(1). NAK 프레임을 받았을 경우.

- 만약, 수신측으로 0~5까지의 데이터를 보냈다고 가정하자.
- 수신측에서 데이터를 받았음을 확인하는 ACK 프레임을 중간 중간 보내게 되며, ACK 프레임을 확인한 전송측은 계속해서 데이터를 전송한다.
- 그러나 만약 수신측에서 데이터 오류 프레임 2를 발견하고 NAK2를 전송측에 보낸다고 해보자.
- NAK2를 받은 전송측은 데이터 프레임 2가 잘못되었다는 것을 알고 데이터를 재전송한다.
- **GBn ARQ의 특징은 데이터를 재전송하는 부분이다. NAK(n)를 받아 n 데이터 이후의 모든 데이터를 재전송한다.**

(2). 전송 데이터 프레임의 분실

- GBn ARQ의 특징은 확인된 데이터 이후의 모든 데이터 프레임 재전송과 수신측의 폐기이다.
- 수신측에서 데이터 1을 받고 다음 데이터로 3을 받게 된다면 데이터 2를 받지 못했으므로 수신측에서는 데이터 3을 폐기하고 데이터 2를 받지 못했다는 NAK2를 전송측에 보낸다.
- NAK를 받은 전송측은 (1) 경우와 같이 NAK(n) 데이터로부터 모든 데이터를 재전송하며 수신측은 기존에 받았던 데이터 중 NAK(n)으로 보냈던 대상 데이터 이후의 모든 데이터를 폐기하고 재전송 받는다.

[![img](https://user-images.githubusercontent.com/33534771/75339152-507e0000-58d3-11ea-876e-e29653f9f99e.png)](https://user-images.githubusercontent.com/33534771/75339152-507e0000-58d3-11ea-876e-e29653f9f99e.png)

(3). 지정된 타임 아웃 내의 ACK 프레임 분실(Lost ACK)

- 전송측은 분실된 ACK를 다루기 위해 타이머를 가지고 있다.
- 전송측에서는 이 타이머의 타임 아웃 동안 수신측으로부터 ACK 데이터를 받지 못했을 경우, 마지막 ACK된 데이터부터 재전송한다.

[![img](https://user-images.githubusercontent.com/33534771/75339203-6ab7de00-58d3-11ea-973d-a4db4abf52a5.png)](https://user-images.githubusercontent.com/33534771/75339203-6ab7de00-58d3-11ea-973d-a4db4abf52a5.png)

- 전송측은 NAK ㅍ프레임을 받았을 경우, NAK 프레임 번호부터 데이터를 재전송한다.
- 수신측은 원하는 프레임이 아닐 경우, 데이터를 모두 폐기 처리한다.
- 타임아웃(ACK 분실)의 경우, 마지막 ACK된 데이터부터 재전송한다.

1. SR(Selective-Reject) ARQ

- GBn ARQ의 확인된 마지막 프레임 이후의 모든 프레임을 재전송하는 단점을 보완한 기법이다.
- SR ARQ는 손상된, 손실된 프레임만 재전송한다.
- 그렇기 때문에 별도의 데이터 재정렬을 수행해야 하며, 별도의 버퍼를 필요로 한다.
- 수신측에 버퍼를 두어 받은 데이터의 정렬이 필요하다.

[![img](https://user-images.githubusercontent.com/33534771/75339302-963ac880-58d3-11ea-9a7f-fe6e4a7a6f38.png)](https://user-images.githubusercontent.com/33534771/75339302-963ac880-58d3-11ea-9a7f-fe6e4a7a6f38.png)

## 혼잡 제어

- 송신측의 데이터 전달과 네트워크의 데이터 처리 속도를 해결하기 위한 기법이다.
- 한 라우터에게 데이터가 몰려 모든 데이터를 처리할 수 없는 경우, 호스트들은 재전송을 하게 되고 결국 혼잡만 가중시켜 오버플로우나 데이터 손실이 발생한다.
- 이러한 **네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송 속도를 제어**하는 것이 혼잡 제어의 개념이다.

1. `AIMD(Additive Increase Multicative Decrease)`

- 합 증가 / 곱 감소 알고리즘이라고 한다.
- 처음에 패킷 하나를 보내는 것으로 시작하여 전송한 패킷이 문제 없이 도착한다면 Window Size를 1씩 증가시키며 전송하는 방법이다. 만약, 패킷 전송을 실패하거나 TIME_OUT이 발생하면 Window Size를 절반으로 감소시킨다.
- 이 방식은 공평하다.
  - 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형 상태로 수렴하게 되는 특징이 있다.
- 문제점은 초기 네트워크의 높은 대역폭을 사용하지 못하고 네트워크가 혼잡해지는 상황을 미리 감지하지 못하여 혼잡해지고 나서야 대역폭을 줄이는 방식이다.

1. `Slow Start`

- AIMD가 네트워크의 수용량 주변에서는 효율적으로 동작하지만, 처음에 전송 속도를 올리는 데 시간이 너무 길다는 단점이 있다.
- Slow Start는 AIMD와 마찬가지로 패킷을 하나씩 보내는 것부터 시작한다. 이 방식은 패킷이 문제 없이 도착하면 각각의 ACK 패킷마다 Window Size를 1씩 늘린다. 즉, 한 주기가 지나면 Window Size는 2배가 된다.
- 따라서 그래프의 모양은 지수 함수 꼴이 된다.
- 혼잡 현상이 발생하면 Window Size를 1로 떨어뜨린다.
- 처음에는 네트워크의 수용량을 예측할 수 있는 정보가 없지만 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느정도 예상할 수 있으므로 혼잡 현상이 발생하였던 Window Size의 절반까지는 이전처럼 지수 함수 꼴로 Window Size를 증가시키고 그 이후부터는 완만하게 1씩 증가시키는 방식이다.

[![img](https://user-images.githubusercontent.com/33534771/75339337-a6eb3e80-58d3-11ea-8ff1-b99bcc992fca.png)](https://user-images.githubusercontent.com/33534771/75339337-a6eb3e80-58d3-11ea-8ff1-b99bcc992fca.png)

- 미리 정해진 임계값(threshold)에 도달할 때까지 윈도우의 크기를 2배씩 증가시킨다.
- Slow Start라는 이름을 사용하지만, 매 전송마다 2배씩 증가하기 때문에 전송되어지는 데이터의 크기는 지수함수적으로 증가한다.
- 전송되는 데이터의 크기가 임계 값에 도달하면 혼잡 회피 단계로 넘어간다.

[![img](https://user-images.githubusercontent.com/33534771/75339370-bb2f3b80-58d3-11ea-9211-af3ca1e5960b.png)](https://user-images.githubusercontent.com/33534771/75339370-bb2f3b80-58d3-11ea-9211-af3ca1e5960b.png)

**[혼잡 회피(Congestion Avoidance)]**

- 윈도우의 크기가 임계 값에 도달한 이후에는 데이터의 손실이 발생할 확률이 높아진다.
- 따라서 이를 회피하기 위해 **윈도우 크기를 선형적으로 1씩 증가시키는 방법**이다.
- 수신측으로부터 일정 시간 동안까지 ACK를 수신하지 못하는 경우.
  - 타임 아웃의 발생 : 네트워크 혼잡이 발생했다고 인식.
  - 혼잡 상태로 인식된 경우
    - 윈도우의 크기를 즉, 세그먼트의 수를 1로 감소시킨다.
    - 동시에 임계값을 패킷 손실이 발생했을 때의 윈도우 크기의 절반으로 줄인다.

**[빠른 회복(Fast Recovery)]**

- 혼잡한 상태가 되면 Window Size를 1로 줄이지 않고 절반으로 줄이고 선형 증가시키는 방법이다.
- 빠른 회복 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 된다.

**[빠른 재전송(Fast Retransmit)]**

- 수신측에서 패킷을 받을 때 먼저 도착해야 할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보낸다. 단, 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보낸다. 따라서 중간에 패킷 하나가 손실되면 송신측에서는 순번이 중복된 ACK 패킷을 받게 된다. 이것을 감지하면 문제가 되는 순번의 패킷을 재전송할 수 있다.
- 빠른 재전송은 중복된 순번의 패킷을 3개(3 ACK) 받으면 재전송한다. 그리고 이러한 현상이 일어나는 것은 약간의 혼잡이 발생한 것으로 간주하여 Window Size를 절반으로 줄인다.

<br/>

#### 1. 흐름제어 (Flow Control)

- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제없지만, 송신측의 속도가 빠를 경우 문제가 생긴다.

- 수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번히 발생한다.

- 이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야한다.

- 해결방법

  - Stop and Wait : 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

    - [![img](https://camo.githubusercontent.com/cb7f08015fa52f106d69a4cab2c4ad48129e2b71133e368808c51578c01f5437/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323633423744344535373135454345423332)](https://camo.githubusercontent.com/cb7f08015fa52f106d69a4cab2c4ad48129e2b71133e368808c51578c01f5437/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323633423744344535373135454345423332)

  - Sliding Window (Go Back N ARQ)

    - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어기법

    - 목적 : 전송은 되었지만, acked를 받지 못한 byte의 숫자를 파악하기 위해 사용하는 protocol

      LastByteSent - LastByteAcked <= ReceivecWindowAdvertised

      (마지막에 보내진 바이트 - 마지막에 확인된 바이트 <= 남아있는 공간) ==

      (현재 공중에 떠있는 패킷 수 <= sliding window)

  - 동작방식 : 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송

    - [![img](https://camo.githubusercontent.com/3cf70c635be033188c4f4b945b6a873100ea1edc6c6c43304b54d896a5385441/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323533463745343835373135454435463237)](https://camo.githubusercontent.com/3cf70c635be033188c4f4b945b6a873100ea1edc6c6c43304b54d896a5385441/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323533463745343835373135454435463237)

  - Window : TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 '3 way handshaking'을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 된다.

  - 세부구조

    1. 송신 버퍼
       - [![img](https://camo.githubusercontent.com/5300c92ecb92341c1da329dc259bc28bfa7dc907928a46fee2090cf1a3cc4aa6/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323235333246343835373135454446323138)](https://camo.githubusercontent.com/5300c92ecb92341c1da329dc259bc28bfa7dc907928a46fee2090cf1a3cc4aa6/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323235333246343835373135454446323138)-
       - 200 이전의 바이트는 이미 전송되었고, 확인응답을 받은 상태
       - 200 ~ 202 바이트는 전송되었으나 확인응답을 받지 못한 상태
       - 203 ~ 211 바이트는 아직 전송이 되지 않은 상태
    2. 수신 윈도우
       - [![img](https://camo.githubusercontent.com/44ccd747b75539c822ab51cd4b977db3493cca0b4c7054a98cb7edda4a5ddaf6/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323534303341343835373135454533363242)](https://camo.githubusercontent.com/44ccd747b75539c822ab51cd4b977db3493cca0b4c7054a98cb7edda4a5ddaf6/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323534303341343835373135454533363242)
    3. 송신 윈도우
       - [![img](https://camo.githubusercontent.com/7720b92250e9083fce77e1ded3b009873cb992cf49bbe601aa29b9691595eea9/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323532303234344235373135454536413134)](https://camo.githubusercontent.com/7720b92250e9083fce77e1ded3b009873cb992cf49bbe601aa29b9691595eea9/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323532303234344235373135454536413134)
       - 수신 윈도우보다 작거나 같은 크기로 송신 윈도우를 지정하게되면 흐름제어가 가능하다.
    4. 송신 윈도우 이동
       - [![img](https://camo.githubusercontent.com/a1ee8c02baf86e6a58b17375d367cd3ad69818cc95b61f8cf44fd53492c850fb/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323237444338353035373135454542413041)](https://camo.githubusercontent.com/a1ee8c02baf86e6a58b17375d367cd3ad69818cc95b61f8cf44fd53492c850fb/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323237444338353035373135454542413041)
       - Before : 203 ~ 204를 전송하면 수신측에서는 확인 응답 203을 보내고, 송신측은 이를 받아 after 상태와 같이 수신 윈도우를 203 ~ 209 범위로 이동
       - after : 205 ~ 209가 전송 가능한 상태
    5. Selected Repeat



#### 2. 혼잡제어 (Congestion Control)

- 송신측의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달된다. 만약 한 라우터에 데이터가 몰릴 경우, 자신에게 온 데이터를 모두 처리할 수 없게 된다. 이런 경우 호스트들은 또 다시 재전송을 하게되고 결국 혼잡만 가중시켜 오버플로우나 데이터 손실을 발생시키게 된다. 따라서 이러한 네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송속도를 강제로 줄이게 되는데, 이러한 작업을 혼잡제어라고 한다.
- 또한 네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라 하며, 혼잡 현상을 방지하거나 제거하는 기능을 혼잡제어라고 한다.
- 흐름제어가 송신측과 수신측 사이의 전송속도를 다루는데 반해, 혼잡제어는 호스트와 라우터를 포함한 보다 넓은 관점에서 전송 문제를 다루게 된다.
- 해결 방법
  - [![img](https://camo.githubusercontent.com/e5daaf381dd565e77a2827a78e339a708cb239e302354ff75b446f39e0134286/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323536453339343235373135463130313033)](https://camo.githubusercontent.com/e5daaf381dd565e77a2827a78e339a708cb239e302354ff75b446f39e0134286/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323536453339343235373135463130313033)
  - AIMD(Additive Increase / Multiplicative Decrease)
    - 처음에 패킷을 하나씩 보내고 이것이 문제없이 도착하면 window 크기(단위 시간 내에 보내는 패킷의 수)를 1씩 증가시켜가며 전송하는 방법
    - 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷의 보내는 속도를 절반으로 줄인다.
    - 공평한 방식으로, 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형상태로 수렴하게 되는 특징이 있다.
    - 문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지하지 못한다. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식이다.
  - Slow Start (느린 시작)
    - AIMD 방식이 네트워크의 수용량 주변에서는 효율적으로 작동하지만, 처음에 전송 속도를 올리는데 시간이 오래 걸리는 단점이 존재했다.
    - Slow Start 방식은 AIMD와 마찬가지로 패킷을 하나씩 보내면서 시작하고, 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size를 1씩 늘려준다. 즉, 한 주기가 지나면 window size가 2배로 된다.
    - 전송속도는 AIMD에 반해 지수 함수 꼴로 증가한다. 대신에 혼잡 현상이 발생하면 window size를 1로 떨어뜨리게 된다.
    - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느 정도 예상할 수 있다.
    - 그러므로 혼잡 현상이 발생하였던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후부터는 완만하게 1씩 증가시킨다.
  - Fast Retransmit (빠른 재전송)
    - 빠른 재전송은 TCP의 혼잡 조절에 추가된 정책이다.
    - 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보내게 된다.
    - 단, 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보내게 되므로, 중간에 하나가 손실되게 되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 된다. 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있다.
    - 중복된 순번의 패킷을 3개 받으면 재전송을 하게 된다. 약간 혼잡한 상황이 일어난 것이므로 혼잡을 감지하고 window size를 줄이게 된다.
  - Fast Recovery (빠른 회복)
    - 혼잡한 상태가 되면 window size를 1로 줄이지 않고 반으로 줄이고 선형증가시키는 방법이다. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 된다.



#### Reference

- http://d2.naver.com/helloworld/47667
- http://asfirstalways.tistory.com/327
- https://www.geeksforgeeks.org/why-does-dns-use-udp-and-not-tcp/
- https://support.microsoft.com/en-us/help/556000
- https://www.brianstorti.com/tcp-flow-control/
- https://www.brianstorti.com/tcp-flow-control/
- [TCP/UDP\] TCP와 UDP의 특징과 차이](https://mangkyu.tistory.com/15)
- [TCP가 연결을 생성하고 종료하는 방법, 핸드쉐이크](https://evan-moon.github.io/2019/11/17/tcp-handshake/)
- [RFC793, TCP Specification](https://tools.ietf.org/html/rfc793#section-3.4)
- [정보통신기술용어해설, UDP](http://www.ktword.co.kr/abbr_view.php?m_temp1=323)
- [정보통신기술용어해설, TCP](http://www.ktword.co.kr/abbr_view.php?nav=2&choice=map&id=428&m_temp1=347)
- [Wikipedia, UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol)
- [Difference between TCP and UDP?](https://stackoverflow.com/questions/5970383/difference-between-tcp-and-udp)
- [TCP와 UDP 비교 정리](http://swalloow.tistory.com/77)
- [[Networking\] TCP 대신 UDP를 사용하는 것이 적절한가요?](https://code.i-harness.com/ko-kr/q/10c798)
- [TCP와 UDP의 차이점](http://jjalidev.blogspot.com/2013/10/tcp-udp.html)
- [TCP/IP, TCP, UDP](http://egloos.zum.com/Esunny/v/4050771)
- [TCP/UDP의 포트 목록](https://ko.wikipedia.org/wiki/TCP/UDP의_포트_목록)
- [65,535 or 65,536 ports?](http://www.antionline.com/showthread.php?266244-65-535-or-65-536-ports)





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

[
![img](https://camo.githubusercontent.com/44fe40125073c05a22961a70ae08c4e0b6b2bf5bfbd11f5ab9135e2a683241a7/68747470733a2f2f73373238302e7063646e2e636f2f77702d636f6e74656e742f75706c6f6164732f323031382f30362f6f73692d6d6f64656c2d372d6c61796572732d312e706e67)](https://camo.githubusercontent.com/44fe40125073c05a22961a70ae08c4e0b6b2bf5bfbd11f5ab9135e2a683241a7/68747470733a2f2f73373238302e7063646e2e636f2f77702d636f6e74656e742f75706c6f6164732f323031382f30362f6f73692d6d6f64656c2d372d6c61796572732d312e706e67)



#### 7계층은 왜 나눌까?

통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문이다.



##### 1) 물리(Physical)

> 리피터, 케이블, 허브 등

단지 데이터 전기적인 신호로 변환해서 주고받는 기능을 진행하는 공간

즉, 데이터를 전송하는 역할만 진행한다.



##### 2) 데이터 링크(Data Link)

> 브릿지, 스위치 등

물리 계층으로 송수신되는 정보를 관리하여 안전하게 전달되도록 도와주는 역할

Mac 주소를 통해 통신한다. 프레임에 Mac 주소를 부여하고 에러검출, 재전송, 흐름제어를 진행한다.



##### 3) 네트워크(Network)

> 라우터, IP

데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당한다.

라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달해준다.

라우팅, 흐름 제어, 오류 제어, 세그먼테이션 등을 수행한다.



##### 4) 전송(Transport)

> TCP, UDP

TCP와 UDP 프로토콜을 통해 통신을 활성화한다. 포트를 열어두고, 프로그램들이 전송을 할 수 있도록 제공해준다.

- TCP : 신뢰성, 연결지향적
- UDP : 비신뢰성, 비연결성, 실시간



##### 5) 세션(Session)

> API, Socket

데이터가 통신하기 위한 논리적 연결을 담당한다. TCP/IP 세션을 만들고 없애는 책임을 지니고 있다.



##### 6) 표현(Presentation)

> JPEG, MPEG 등

데이터 표현에 대한 독립성을 제공하고 암호화하는 역할을 담당한다.

파일 인코딩, 명령어를 포장, 압축, 암호화한다.



##### 7) 응용(Application)

> HTTP, FTP, DNS 등

최종 목적지로, 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.

사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스를 제공한다.

<br/>

[![img](https://user-images.githubusercontent.com/33534771/74589801-e603cf00-504b-11ea-862c-765c57d3169b.png)](https://user-images.githubusercontent.com/33534771/74589801-e603cf00-504b-11ea-862c-765c57d3169b.png)

**OSI 7 계층을 나누는 이유는 무엇일까?**

중요한 목적은 표준과 학습 도구라 할 수 있다. 표준화를 통해 이질적인 포트 문제나 프로토콜 등으로 인한 문제를 해결하여 비용을 절감했다. 또한, 계층별의 기능과 통신 과정을 단계별로 나누어서 쉽게 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문에 편리하다.

**1) 물리(Physical)**

> 리피터, 케이블, 허브 등

주로 전기적, 기계적, 기능적인 특성을 이용해서 통신 케이블로 데이터를 전송하는 역할을 한다.

**2) 데이터 링크(Data Link)**

> 브릿지, 스위치 등

물리 계층을 통해 송, 수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 도와주는 역할을 한다.

MAC 주소를 이용해 통신한다.

Frame에 MAC 주소를 부여하고 에러검출, 재전송, 흐름 제어를 진행한다.

**3) 네트워크(Network)**

> 라우터, IP

여러 개의 노드를 거칠 때마다 경로를 찾아주는 역할을 하며, 다양한 길이의 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당한다. (전송 계층이 요구하는 서비스 품질을 제공하기 위한 기능적, 절차적 수단을 제공한다.)

라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달해준다.

라우팅, 흐름 제어, 오류 제어, 세그먼테이션 등을 수행한다.

**4) 전송 계층(Transport)**

> TCP, UDP

TCP, UDP 프로토콜을 통해 통신을 활성화 한다. 포트를 열어두고, 프로그램들이 전송을 할 수 있도록 제공해준다. 이를 통해 양 끝 단의 사용자들이 데이터를 주고 받을 수 있다.

- TCP : 신뢰성, 연결 지향적
- UDP : 비신뢰성, 비연결성, 실시간

**5)세션(Session)**

> API, Socket

양 끝 단의 응용 프로세스가 통신을 관리하기 위한 방법을 제공한다.

데이터가 통신하기 위한 논리적 연결을 담당한다.

TCP/IP 세션을 만들고 없애는 책임을 지니고 있다.

**6) 표현(Presentation)**

> JPEG, MPEG 등

데이터 표현에 대한 독립성을 제공하고 암호화하는 역할을 담당한다.

코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어준다.

파일 인코딩, 명령어를 포장, 압축, 암호화한다.

**7) 응용(Application)**

> HTTP, FTP, DNS 등

최종 목적지로 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.

사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스를 제공한다.



<br/>

<br/>

## OSI 7계층

- 국제 표준화 기구(ISO)가 1984년 발표했다.
- 통신이 일어나는 과정을 7단계로 구분하여 한눈에 들어올 수 있도록 작성했다.
- 컴퓨터 통신 구조의 모델과 앞으로 개발될 프로토콜의 표준적인 뼈대를 제공하기 위해 개발된 참조 모델이다.

------

## TCP/IP 모델

- 미국에서 개발한 인터넷의 기본 통신 프로토콜이다.
- DOM Model(미국방성 모델)을 기반으로 개발했다.
- TCP
  - **연결 지향형 프로토콜**이다.
  - 세션의 연결과 종료, [흐름제어](https://goodgid.github.io/Error-Flow-Control/#흐름-제어), [혼잡 제어](https://goodgid.github.io/Error-Flow-Control/#혼잡-제어), [오류제어](https://goodgid.github.io/Error-Flow-Control/#오류-제어), 패킷의 분할 및 재조립에 사용한다.
- IP
  - **비 연결지향형 프로토콜**
  - **데이터 전송**에 사용한다.
- OSI 7 Layer는 장비 개발자들이 어떻게 표준을 잡을지 결정하기 위해 많이 사용한다.
  (구조가 나뉘어 있어 공부 할 때 많이 사용한다.)
- TCP/IP는 현재 **실질적**으로 사용되고 있는 **프로토콜**이다.
  (일반적으로 널리 퍼져있으며 편리한 점이 있다.)

![img](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_3.png)

------

## OSI 7 Layer Model

- 데이터 흐름이 한눈에 보인다.
- 계층을 7단계로 구분하고 각 **층별**로 **표준화**를 했기 때문에 여러 회사 장비를 사용해도 네트워크에 이상이 없다.
- 상위 계층(7,6,5)과 하위 계층(4,3,2,1)으로 나눈다.

### 7 계층 : Application Layer

- 응용 계층(Application Layer)
- **사용자 인터페이스**의 역할을 담당하는 계층이다.
- 즉, 사용자가 이용하는 **네트워크** 응용프로그램이다.
  EX) Internet Explorer
- 사용자와 가장 가까운 프로토콜을 정의한다.
  EX) HTTP(80), FTP(20,21), Telnet(23), SMTP(25), DNS(53), TFTP(69) 등

------

### 6 계층 : Presentation Layer

- 표현 계층(Presentation Layer)
- **전송**하는 **데이터의 Format**(구성 방식)을 결정하는 계층이다.
- 다양한 데이터 Format을 **일관되게 상호 변환**, **압축** 및 **암-복호화** 기능을 수행한다.
  EX) ASCII, EBCDIC, CIF, JPEG, AVI, MPEG 등

------

### 5 계층 : Session Layer ★

- 세션 계층(Session Layer)
- 네트워크 상에서 통신을 할 경우 양쪽 **host간 최초 연결**이 되게 하고
  통신 중 **연결**이 **지속**되도록 시켜주는 역할을 하는 계층이다.
- 통신을 하는 두 host사이에 **세션**을 열고, 닫고, 관리하는 기능을 담당한다.
- **중요한 기능**에는 **동기화**와 **대화 기능**이 있다.
- **동기**란 통신 양단에서 서로 동의하는 논리적인 공통처리 지점으로써, **동기점**을 **설정**하기위해 사용된다.
- 동기점은 **오류 복구**를 위하여 필수적으로 사용되는데
  **동기점이 설정**된다는 의미는 그 **이전까지의 통신**은 서로 **완벽하게 처리**했다는것을 뜻한다.
- 동기점 이전 과정은 복구가 필요 없고 동기점 **이후 처리 과정**에 대한 **복구 절차**가 진행된다.
- **대화**는 **데이터 전송 과정**을 의미한다.
- **시간 경과**에 따라 **순차적**으로 **동기점**을 **부여**하여 **신뢰성 보장 기능**을 **단계적**으로 **구현**할 수 있게 된다.
- **의도적**으로 **일시 정지**하여 나중에 이어서 작업을 하는 것이 가능하다.
- 데이터 송수신 방식(Duplex), 반 이중 방식(Half Duplex), 전 이중 방식(Full Duplex)의 통신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정을 수행한다.
- 대표적인 프로토콜로는 **SSL/TLS**가 있다.
- TCP, IP, IPX 등 **전송계층 프로토콜**을 **연결**해주는 역할을 한다. 연결을 도와주는 거다.

------

> [7 6 5] 계층을 통하여 Data가 만들어 진다.

------

### 4 계층 : Transport Layer

- 전송 계층(Transport Layer)
- **정보**를 **분할**하고, 상대편에 **도달하기 전**에 **다시 합치는 과정**을 담당하는 계층이다.
- 4계층의 단위 : **Segment**
- 목적지 컴퓨터에서 발신지 컴퓨터 간의 통신에 있어 **[흐름제어](https://goodgid.github.io/Error-Flow-Control/#흐름-제어)**, **[혼잡 제어](https://goodgid.github.io/Error-Flow-Control/#혼잡-제어)**, **[오류제어](https://goodgid.github.io/Error-Flow-Control/#오류-제어)**를 담당한다.
- **전송 방식**을 결정한다.
  EX) 포트번호나 TCP/UDP 등
- 4계층 프로토콜 : **TCP**, **UDP**
  - **TCP** - 신뢰성, 연결지향성 프로토콜, Connection-ful(연결을 유지하며 전송하는 방식)
  - **UDP** - 비 신뢰성, 비 연결지향성 프로토콜, Connection-less(연결을 유지하지 않고 전송하는 방식, Data 손실을 신경쓰지 않음)

------

### 3 계층 : Network Layer

- 네트워크 계층(Network Layer)
- **Logical Address**(IP, IPX)를 담당한다.
- **패킷**의 **이동경로**를 결정하는 계층이다.
- 3계층의 단위 : **Packet**
- **경로선택**, **라우팅**, **논리적인주소(IP)**를 정의하는 계층이다.
- Routing Protocol을 이용하여 최적경로를 선택한다.
- 대표적인 프로토콜로는 VPN을 구성하는데 사용되는 **IPSec**가 있다.
- 3계층 장비 : **Router**

------

### 2 계층 : Data Link Layer

- 데이터링크 계층(Data Link Layer)

- **물리적 계층**을 통한 **데이터 전송**에 **신뢰성**을 제공한다.

- 이러한 서비스를 위해 물리적 주소(MAC) 지정, 네트워크 토폴로지, 오류통지, 프레임의 순차적 전송, 흐름제어 등의 기능을 가진다.

- 직접 연결되어 있지 않은 네트워크에 대해서는 **상위 계층**에서 **오류 제어**를 담담한다.

- 두가지 하위 게층

  이 존재한다.

  - **Logical Link Contorl** - 통신 장치간의 연결을 설정하고 관리하는 책임
  - **Media Access Control** - 다중 장치가 같은 미디어 채널을 공유, 제어(Block ID + Device ID)

- 2계층 장비 : **Switch**, **Bridge**

------

### 1 계층 : Physical Layer

- 물리 계층(Physical Layer)
- 네트워크 통신을 위한 **물리적인 표준**을 정의하는 계층이다.
- 두 컴퓨터 간의 **전기적**, **기계적**, **절차적인 연결**을 정의하는 계층이다.
- 케이블 종류, 데이터 송수신 속도, 신호의 전기 전압 등

![img](https://goodgid.github.io/assets/img/network/osi_7_layer_2.png)

------

> Q. 5계층에서 생성하는 세션이랑 TCP에서 이뤄지는 3-Way & 4-Way Handshake랑 무슨차이지?

- 5계층에서 하는 것은 종단 프로세스(프로세스)세션이라 생각하면 된다. (생성 유지 종료)
- TCP에서 이뤄지는 행위는 서버와 클라이언트의 관계라 생각하면 된다.

<br/>

-----



-------



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

