## HTTP란?

> HyperText Transfer Protocol
> 
- HyperText란?

> 사용자에게 내용의 비순차적 검색이 가능하도록 제공되는 텍스트. 문서 내의 특정한 단어가 다른 단어나 데이터베이스와 링크되어 있어 사용자가 관련 문서를 넘나들며 원하는 정보를 얻을 수 있음.
> 

 결국 하이퍼 텍스트를 전송하는 프로토콜

HTTP 메시지에 모든 것을 전송

- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

### HTTP의 역사

- HTTP/1.1 1997년: 가장 많이 사용
    - RFC2068 (1997) → RFC2616 (1999) → RFC7230 ~ 7235 (2014) `RFC는 버전`
- HTTP/2 2015년: 성능 개선
- HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선

### 기반 프로토콜

- TCP: HTTP/1.1, HTTP/2
- UDP: HTTP/3
- 현재 HTTP/1.1을 많이 사용
    - HTTP/2, HTTP/3 도 점점 증가
    

## 클라이언트 서버 구조

- Request, Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답
- 서버는 비즈니스 로직하고 데이터에만 집중
- 클라이언트는 UI를 그리는 것에만 집중

## 무상태 프로토콜 (stateless)

- 서버가 클라이언트 상태를 보존X
- 장점: 서버 확장성 높음 (스케일 아웃)
- 단점: 클라이언트가 추가 데이터 전송 (데이터를 한번에 너무 많이 보냄)

### Stateful

- 서버가 클라이언트의 이전 상태를 보존함.
- 항상 같은 서버가 유지되어야 한다. → 무한히 서버를 증설할 수 없다.
- 중간에 서버에 장애가 발생하면 클라이언트 1은 처음부터 다시 실행해야한다

### Stateless

- 서버가 클라이언트의 이전 상태를 보존하지 않음.
- 무 상태는 응답 서버를 쉽게 바꿀 수 있다. → 무한한 서버 증설 가능 (스케일 아웃 - 수평 확장에 유리)
- 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.
- 무상태
    - 예) 로그인이 필요 없는 단순한 서비스 소개 화면
- 상태 유지
    - 로그인
- 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용

## 비연결성

### 연결을 유지하는 모델

- TCP/IP는 기본적으로 연결을 유지함.
- 서버는 연결을 계속 유지, 서버 자원의 지속 소모

### 연결을 유지하지 않는 모델

- 서버는 연결을 유지X, 최소한의 자원 유지

- HTTP는 기본적으로 연결을 유지하지 않는 모델
- 일반적으로 초 단위의 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
    - 예) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
- 서버 자원을 매우 효율적으로 사용할 수 있음

### 한계와 극복

- TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등등 수 많은 자원이 함께 다운로드
- 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결
- HTTP/2, HTTP/3에서 더 많은 최적화

### HTTP 초기 - 연결, 종료 낭비

- 클라이언트 자원을 통신할 때마다 매번 연결과 종료를 반복함
- HTTP 지속 연결 (Persistent Connections)는 자원을 다 받을 때까지 연결을 유지함.

## HTTP 메시지

- HTTP 기본 메시지 구조
    - start-line 시작 라인
    - header 헤더
    - empty line 공백 라인(CRLF)
    - message body
- HTTP 요청 메시지
    - start-line: GET/search?q=hello&hl=ko HTTP/1.1
        - request-line / status-line
    - header: Host: www.google.com
    - empty line: 공백
    - body: message body (기본적으로 없으나 가질 수 있음)
- HTTP 응답 메시지
    - start-line: HTTP/1.1 200 OK
    - header: Content Type과, Content-Length이 포함 됨
    - empty line: 공백
    - message: 기본적으로 html이나 html 본문이 들어감

### HTTP message의 공식 스펙

- HTTP-message =
    
    ```swift
    start-line
    *(header-field CRLF)
    CRLF
    [ message-body ]
    ```
    

## 시작 라인

### 요청 메시지

- start-line = request-line / status-line
- request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
- HTTP 메서드 (GET: 조회)
    - 종류: GET, POST, PUT, DELETE...
    - 서버가 수행해야 할 동작 지정
        - GET: 리소스 조회
        - POST: 요청 내역 처리
- 요청 대상 (/search?q=hello&hl=ko)
    - absolute-path[?query] (절대경로[?쿼리])
    - 절대경로 = "/"로 시작하는 경로
- HTTP Version

### 응답 메시지

- status-line = HTTP-version SP status code SP reason-phrase CRLF
- HTTP 버전
- HTTP 상태 코드: 요청 성공, 실패를 나타냄
    - 200: 성공
    - 400: 클라이언트 요청 오류
    - 500: 서버 내부 오류
- 이유 문구: 사람이 이해할 수 있는 짧은 상태의 코드 설명 글

## HTTP 헤더

- header-field = field-name ":" OWS field-value OWS (OWS: 띄어쓰기 허용)
- field-name은 대소문자 구문 없음, value는 대소문자 구분 함

HTTP 헤더의 용도

- HTTP 전송에 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등...
- message body 빼고 필요한 메타 데이터 정보가 전부 포함 되어 있음
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능
    - helloworld: hihi → 약속한 서버와 클라이언트만 이해할 수 있음.
    

## HTTP 메시지 바디

### 용도

- 실제 전송할 데이터
- HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할수 있는 모든 데이터 전송 가능