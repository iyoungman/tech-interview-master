## TOC

<!-- TOC -->

- [TOC](#toc)
- [TCP 흐름제어, 오류제어 방식](#tcp-흐름제어-오류제어-방식)
- [TCP Connection(Handshake) & Nagle Algorithm](#tcp-connectionhandshake--nagle-algorithm)
- [OSI 7 계층 / TCP 4계층](#osi-7-계층--tcp-4계층)
- [로드밸런싱](#로드밸런싱)
- [네트워크 주소](#네트워크-주소)
- [TCP vs UDP, TCP가 연결형인 이유](#tcp-vs-udp-tcp가-연결형인-이유)
- [웹 브라우저 검색 과정](#웹-브라우저-검색-과정)
- [패킷이란?](#패킷이란)
- [IP](#ip)
- [TCP/IP 4 계층](#tcpip-4-계층)
- [이더넷](#이더넷)
- [HTTP vs HTTPS](#http-vs-https)
- [GET vs POST](#get-vs-post)
- [쿠키 vs 세션](#쿠키-vs-세션)
- [HTTP는 TCP? UDP?](#http는-tcp-udp)
- [HTTP 응답코드](#http-응답코드)
- [쿠키의 속성](#쿠키의-속성)
- [XSS란?](#xss란)
- [POST vs PUT vs PATCH](#post-vs-put-vs-patch)
- [HTTP 헤더](#http-헤더)
- [로드밸런싱. Client Side vs Server Side](#로드밸런싱-client-side-vs-server-side)
- [HTTP Cache](#http-cache)

<!-- /TOC -->

***  

## TCP 흐름제어, 오류제어 방식

* 흐름제어는 `송수신측 속도 차이를 해결`하기 위한 것
    - `정지대기`
        + 하나를 보내고
        + 그것에 대한 응답을 받고
        + 다시 하나를 보내는 방식
        + 비효율
    - `슬라이딩 윈도우`
        + `송수신측 버퍼`
        + 여러 데이터를 한번에 전송 가능
     
* 혼잡제어는 `네트워크의 혼잡도를 제어`하기 위한 것
    - 합 증가/곱 감소
    - 슬로우 스타트
      
* 오류제어는 **흐름제어**와 관련이 있다
    - 기본적으로 `재전송`(ARQ)
    - 정지대기 ARQ
        + 흐름제어의 정지대기에서 사용
        + `ACK 대신 NAK를 받으면 재전송`
    - 슬라이딩 윈도우 : Go-Back-N-ARQ
        + `확인된 마지막 프레임 이후 모두 재전송`
        + 즉, 분실된 프레임 이후
    - 슬라이딩 윈도우 : Selective-Repeat-ARQ(선택적 재전송)
        + `분실된 프레임만 재전송`

* 이는 TCP의 신뢰성을 위함이다

* 참고로 프레임은 데이터링크 계층의 단위이다

***  

## TCP Connection(Handshake) & Nagle Algorithm
* https://gmlwjd9405.github.io/2018/09/19/tcp-connection.html
* https://asfirstalways.tistory.com/356

* 3 Way Handshaking
    + TCP연결은 양방향성이다
    + 따라서 2 Way로는 부족하다
* 4 Way Handshaking
    + Client -> Server Fin
    + Server -> Client Ack
    + Time-Out (전송하고 있는 데이터 있을수도 있다)
    + Server -> Client Fin
    + Client -> Server Ack
    + Server close()

* Nagle Algorithm
    - 기본적으로 연결은 3 Way Handshaking
    - `간단한 데이터를 매번 3 Way로 맺으면 비효율`
    - `송신측에 버퍼를 둔 뒤 한번에 전송`
    - `지연 전송`
    - 전송속도가 느릴 수 있다
    - 부하는 감소한다

***  

## OSI 7 계층 / TCP 4계층
* https://goitgo.tistory.com/25
* [동작 과정](https://galid1.tistory.com/101)  

* OSI 7 계층
    - 네트워크 통신의 개념적 표준
    - 1) `물리`
        + 데이터를 물리 매체상 전송
    - 2) `데이터링크`
        + 물리 계층의 오류를 찾는다
    - 3) `네트워크`
        + 라우팅(데이터의 최적 경로를 찾는것)
    - 4) `전송`
        + 오류 제어
        + 흐름 제어
        + 패킷 재조립
    - 5) `세션`
        + 데이터 통신의 논리적 연결
    - 6) `표현`
        + 암호화
        + 데이터 표현 방식 바꾸는 기능
        + 해당 데이터가 Text 인지, JPG 인지 확인
    -  7) `응용`
        + 사용자와 인터페이스
        + HTTP 프로토콜

* TCP 4 계층
    - `인터넷 통신규약`
    - 실제 사용
    - 1) `네트워크 엑세스`
        + 물리적 연결 지원
    - 2) `인터넷`
        + IP 패킷의 헤더를 만든다
    - 3) `전송`
        + 신뢰성 있는 전송기능
        + TCP or UDP
    - 4) `응용`
        + 사용자 인터페이스

* 프로토콜
    - 응용 계층
        + HTTP, DNS
    - 전송 계층
        + TCP, UDP
    - 인터넷 계층
        + IP
    - 데이터 링크
        + 이더넷
        + 회선으로 전송하는 방법


***  

## 로드밸런싱
* https://nesoy.github.io/articles/2018-06/Load-Balancer  
* https://medium.com/@pakss328/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05  
* `여러대의 서버에 균등하게 트래픽을 분배시켜주는 것`
* `서버의 부하를 분산시키는 기술 `
* 여러대의 서버를 구성
* 정적방식과 동적방식이 있다
* `동작과정`
    - DNS 서버에 `도메인에 대한` IP 주소가 아닌
    - 도메인에 대한 `부하 분산 장치 IP 주소를 등록`한다
    - 도메인을 검색하면 부하 분산 장치의 IP로 이동하고
    - 부하 분산 장치는 적절한 방식(라운드 로빈..)으로
    - 균등하게 IP주소를 반환한다
* L4 vs L7 차이
    * `L4는 IP, Port 등을 기반`
    * `L7는 L4 + Http URL 등 Application 기반`
        * 좀 더 정교하다
    * https://real-dongsoo7.tistory.com/42

* L4, L7은 Swith(하드웨어)
    * `네트워크를 연결하는 일을 하는 특수목적의 하드웨어`

***  

## 네트워크 주소
* 공인 IP
    - 실제 주소
* 사설 IP
    - 공인 IP는 부족
    - 공인 IP 공유
    - 유무선 공유기

* IP 클래스
    - Network ID + Host ID

* 네트워크 주소
    - IP주소 + 서브넷 마스크

* 서브넷 마스크
    - IP 클래스는 비효율성을 가지고 있다
    - 따라서 `하나의 네트워크를 작은 단위로 나눠 사용하기 위함이다`

***  

## TCP vs UDP, TCP가 연결형인 이유
* TCP
    - 신뢰성
        + 흐름제어
        + 혼잡제어
        + 오류제어
    - 패킷
    - 느리다
    - `연결형`
        - `순서 보장`
        - `3Way HandShaking이라는 연결 과정`
    - 가상회선방식
        + 논리적 패킷 경로를 배정
        + 따라서 패킷들의 순서가 정확
        + 연결형 서비스

* UDP
    - 비신뢰성
    - 데이터그램
    - 실시간 서비스
    - 빠르다
    - 비연결형
        - `순서 보장X`
    - 경로X
        + 각각의 패킷은 독립적
        + 따라서 패킷들의 순서가 부정확
        + 비연결형 서비스


***  

## 웹 브라우저 검색 과정
1. 웹 브라우저에 'http://www.google.com/google.png'입력
2. 도메인 네임을 DNS서버에서 검색하여 IP 추출
3. HTTP 요청 메시지 생성
    - GET 방식, IP주소...
    - HTTP는 Application 계층
    - Network Layer에서 송신은 위층 -> 수신은 아래층, 아래층 -> 위층
4. TCP 커넥션 생성
    - 3 Way Handsaking
5. 서버는 전송된 정보를 받아 HTTP 메세지 해석
6. 이를 바탕으로 적절한 작업(DB 등등..)을 한 후 HTTP 응답 메시지 생성
7. 원래 컴퓨터로 전송
8. TCP 커넥션 종료
    - 4 Way Handsaking
9. 결과를 웹 브라우저에 보여준다

***  

## 패킷이란?
* `한번에 전송할 데이터의 크기`
* 왜 쪼개서 보낼까?
> 네트워크상에는 하나의 컴퓨터만 있는 것이 아니고 여러 개의 컴퓨터가 있다.
>
> 만약 한번에 보내면 다른 컴퓨터들은 기다려야 할 수 있다.

* 참고
    * https://itmore.tistory.com/entry/패킷이란

## IP
* 인터넷 프로토콜
* 인터넷의 표준 통신방식

## TCP/IP 4 계층
* Internet 계층(2 계층)
    * TCP 7의 네트워크 계층
    * `전송 계층으로 받은 데이터에 IP 헤더를 붙여 IP 패킷 생성`
    * 라우터 가 IP 헤더를 해석해 IP 패킷을 라우팅한다.
    * `라우팅 기능`

* Network Access 계층(1 계층)
    * `TCP/IP 패킷을 네트워크 매체로 전달`
    * Device Driver, Network Hardware
    * http://egloos.zum.com/OpheliaSo/v/3860537

## 이더넷
* `물리 + 데이터링크 계층`
* `CSMA/CD 방식을 통해 통신하는 네트워크 규격`
    * https://security-nanglam.tistory.com/193
* 기기들의 MAC주소를 가지고 통신할 수 있도록 한다.
* 참고
    * https://goodgid.github.io/NW-Ethernet/
    * https://engkimbs.tistory.com/10

* MAC주소란?
    * 기기별로 갖고있는 고유한 주소
    * https://blog.naver.com/PostView.nhn?blogId=shk50611&logNo=221433227927

***

## HTTP vs HTTPS
* HTTP
    * 웹 상에서 클라이언트와 서버 간에 요청/응답(request/response)으로 정보를 주고 받을 수 있는 프로토콜
    * `비연결`
        * 응답 요청후 연결이 끊긴다
    * `무상태`
        * 연결이 끝나면 상태를 유지하지 않는다.
* HTTPS
    * HTTP에 보안 강화
    * 정보를 암호화하여 전송
    * 데이터 암호화 알고리즘
        * 공개키 암호화 방식
        * `암호화키와 복호화키가 다르다.`

***

## GET vs POST
* GET
    * URI를 통해 요청
    * 전송조회 용도
    * 요청시 Body는 비어있다.
    * `Body의 데이터 타입을 표현하는 'Content-Type' 필드도 HTTP Request Header에 들어가지 않는다.`
* POST
    * BODY를 통해 요청
    * 데이터 조작 용도
    * `Content-Type`

* Content-Type
    * 서버에 요청시 POST 방식은 데이터 전송을 기반으로 한 요청방식이다. POST방식은 URL이 아닌 BODY에 데이터를 넣어서 보낸다.
    * 따라서 헤더필드에 BODY의 데이터를 설명하는 Content-Type이라는 헤더 필드가 들어가며 어떤 데이터 타입인지 명시한다.


***

## 쿠키 vs 세션
* 차이점
    * `정보 저장 위치`

* 단점
    * 탈취당했을때 -> 유효시간

* JWT
    * `Stateless`
    * JWT도 Client에게 줄때는 쿠키에 내려준다.
    * 다만 쿠키의 유효시간이 없다.
    * 단점
        * Claim의 정보가 많아질수록 토큰의 길이가 길어질 수 있다.
        토큰은 매 API 호출시마다 헤더에 담기 때문에 비효율적일 수 있다.
        * Claim을 단순 Base64 인코딩하기 때문에 Claim정보가 유출될 수 있다. 이를 보완하기 위해 토큰 자체를 암호화하는 JWE가 있다.
        * 참고
            * https://swiftymind.tistory.com/51
    * 시그니처
        * `앞의 헤더와 페이로드 값, SecretKey를 해시 알고리즘으로 암호화한 값이다.`
        * 왜 암호화할까?
            * `앞의 헤더나 페이로드 값이 위변조 되었는지 확인하기 위해서다.`
        * 물론 토큰 자체가 탈취되고 SecretKey를 알면 Claim은 노출이 될수도 있다.
        * 이를 보완하기 위해 JWE를 사용한다.
        * 참고
            * https://blog.outsider.ne.kr/1160

***

## HTTP는 TCP? UDP?
* HTTP 1, 2는 `기본적으로 TCP 기반`이라고 생각하면 된다.
    * `HTTP는 신뢰할 수 있는 전송방식만 제공하기 때문이다.`
* 가장 최근에 나온 HTTP 3은 UDP 기반이긴하다.

* 그럼 스트리밍 서비스는 HTTP 프로토콜이 아닌걸까?
    * 그렇다.
    * `응용계층의 RTP(Realtime Tansport) 프로토콜을 사용한다.`
    * `RTP 프로토콜은 전송을 UDP 프로토콜을 이용할 것이다.`

* 참고
    * https://stackoverflow.com/questions/31162754/was-http-request-received-over-tcp-or-udp

***

## HTTP 응답코드
> 200번대 성공

> 300번대 리다이렉션 완료

> 400번대 요청 오류
* 400 vs 404
    * 400은 Bad Request
        * `사용자가 잘못된 요청을 했을 때`
            * 예를 들면 파라미터가 잘못되었다던지
            * 유효성 검사를 생각하면 된다.

    * 404는 Not Found
        * `경로가 존재하지 않을 때`
        * `자원이 존재하지 않을 때`

> 500번대 서버 오류

* 참고
    * https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C
    * https://sanghaklee.tistory.com/61


***

## 쿠키의 속성
* Secure
    * HTTPS 프로토콜 상에서 암호화된(encrypted ) 요청일 경우에만 전송

* HTTPOnly
    * HttpOnly쿠키는 JavaScript의 Document.cookie API에 접근할 수 없다.
    * `Document.cookie`
        * JavaScript에서 쿠키를 읽거나 조작할 수 있는 것이다.
    * XSS시에 쿠키에 접근하는 것을 막기 위해서이다.

* https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies
* https://www.crocus.co.kr/1036

* 쿠키는 어떠한 도메인에 대해 파일로 저장되며
* 해당 도메인에 요청될 때 함께 포함되어 요청된다.
* `즉, CSRF는 이런 속성을 이용한 것이다.`
* 악의적인 웹 사이트에서 해당 쿠키 값을 읽을 수는 없지만
* `결국 은행이라는 서버에 요청하는 것이기 때문에 은행서버 쿠키값이 함께 요청된다`

***

## XSS란?
* `XSS 공격은 사이트에 스크립트 코드를 삽입하는 공격기법`

* 해결
1) 사용자 입력이 필요할 경우 클라이언트 단에서 유효성 검사를 하는 방법

2) 서버 단에서 데이터 유효성 검사를 하는 방법

3) innerHTML 속성이 아닌 textContent 속성을 이용하여 마크업이 아닌 `이스케이프로 처리된 텍스트로 코드를 처리할 것`

* innerHTML vs textContent 속성
    * https://m.blog.naver.com/PostView.nhn?blogId=sue227&logNo=221181783645&proxyReferer=https:%2F%2Fwww.google.com%2F

***

## POST vs PUT vs PATCH
* https://iyoungman.github.io/network/POST-PUT-PATCH/

***

## HTTP 헤더
* https://jeong-pro.tistory.com/181
* https://velog.io/@yhe228/HTTP-Header-wbk5i7f8xt

***

## 로드밸런싱. Client Side vs Server Side
* Server Side
    * 일반적인 개념
    * Switch, NGinx..
    * 문제점
        * `Switch자체가 처리할수 있는 요청수에 한계가 있다.`
* Client Side
    * Spring Cloud의 Ribbon이 대표적인 예

* 참고
    * http://blog.leekyoungil.com/?p=259

***

## HTTP Cache

> `기본적으로 Response Header 값을 통해 Cache`

<br>

1) Cache control + Expire

```http
Cache-Control: public, max-age=31536000 
```

* 브라우저는 max-age 동안 서버에 요청 X
* API 서버에서는 사용 X

<br>

2) Last Modified

* Response

```http
Last-Modified: Mon, 03 Jan 2011 17:45:57 GMT
```

* Request
    * 서버에서 받은것을 요청

```http
If-Modified-Since: Mon, 03 Jan 2011 17:45:57 GMT 
```

* 서버
    * `서버는 DB에 요청 API에 대한 수정 시간을 갖고있어야한다.`
    * Client의 요청 시간과 바뀐게 없다면 HTTP 304 Status Code를 내려준다.

<br>

3) ETag
* Last Modified와 비슷하다.
* 시간 대신 Hash값으로 교환.

<br>

* 참고
    * https://medium.com/@bbirec/http-%EC%BA%90%EC%89%AC%EB%A1%9C-api-%EC%86%8D%EB%8F%84-%EC%98%AC%EB%A6%AC%EA%B8%B0-2effb1bfab12
