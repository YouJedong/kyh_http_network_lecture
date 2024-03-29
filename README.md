# kyh_http_network_lecture
김영한의 모든 개발자를 위한 HTTP 웹 기본 지식

(23.02.19 ~)

# 인터넷 네트워크

- 내 PC에서 다른 PC로 인터넷을 통해 데이터를 보낼 때 많은 경로를 거쳐서 보낼텐데 어떤 규칙으로 보내는 것일까?

## 1. IP(인터넷 프로토콜)

### **데이터를 전달하고 받는 순서**

1. 데이터를 전달할 때 패킷이라는 통신 단위로 전달한다.
    - 패킷에 들어가는 정보 : 출발지 IP, 목적지 IP 데이터 등등
2. 인터넷에 노드를 통해 전달전달해서 목적지 IP로 데이터를 보낸다.
3. 데이터를 받은 PC는 성공적으로 받았다는 응답을 다시 전달한다
*데이터를 전달할 때 노드와 응답할 때 노드는 다를 수 있다.

### **IP 프로토콜의 한계**

1. 비연결성 - 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷을 전송한다.
    
    ex) 상대 PC가 꺼져있어도 패킷을 전송한다.
    
2. 비신뢰성 - 중간에 패킷이 사라지거나 여러개의 패킷을 던졌을때 순서대로 오지 않을 수 있다.
3. 프로그램 구분 - 같은 IP를 사용하는 서버에서 통신하는 앱이 둘 이상이면 어디에 데이터를 던져야 하는가?
ex) 한 PC에서 여러 앱(음악, 게임 등)을 실행할 때 같은 IP를 쓰게되는데 어디로 데이터를 보내는가?

> 이 문제를 해결하기 위해 TCP, UDP 프로토콜이 필요하다.
> 

## 2. TCP, UDP

### 인터넷 프로토콜 스택 4계층

1. 애플리 케이션 계층 - HTTP, FTP
2. 전송 계승 - TCP, UDP
3. 인터넷 계층 - IP
4. 네트워크 인터페이스 계층

### 4계층을 거쳐서 전달되는 순서

1. 웹 브라우저, 네트워크게임, 채팅 프로그램 등에서 Socket 라이브러리를 통해 메세지를 넘김
2. OS는 이 메세지를 ***TCP 패킷 정보**로 감싼다.
3. 또 IP 패킷을 생성해서 감싼다.
4. 실제 물리적 LAN 카드로 Ethernet frame으로 마지막으로 감싸서 인터넷으로 전송한다. 

*TCP 패킷 정보

- 출발지 PORT, 목적지 PORT, 전송 제어, 순서 ,검증정보 등등이 들어간다.

### TCP 특징

- 전송 제어 프로토콜
- 연결지향 - ***TCP 3 way handshake** (가상 연결)
- 데이터 전달 보증 - 데이터를 정상적으로 받았는지 응답을 해서 보증을 한다.
- 순서 보장 - 패킷을 순서로 전송했을 때 그 순서대로 안왔을 때 잘못된 순서부터 다시 보내라고 응답을 한다.

### TCP 3 way handshake

<img width="706" alt="tcp_3_way_handshake" src="https://github.com/YouJedong/kyh_http_network_lecture/assets/108327853/de433ccc-f186-476e-834f-e9b64ba5dc03">

1. SYN이라는 요청으로 상대 서버에 접속 요청을 한다.
2. 상대 서버는 요청을 받았으면 SYN+ACK 메세지를 보내면서 접속 요청을 보냄
3. 클라이언트도 ACK를 다시 응답한다.
4. 둘다 접속이 되었을 때 그제서야 데이터를 전송한다.

*물리적으로 연결된 것이 아닌 논리적으로 연결이 되었다~ 라는 것

### UDP - 사용자 데이터그램 프로토콜

- 연결지향, 데이터 전달 보증, 순서 보장 모두 없음
- 대신 단순하고 전송이 빠름
- IP와 거의 같은데 PORT, 체크섬 정도만 추가

### PORT

- 한 PC가 여러개의 서버와 통신하려면?
1. 한 PC가 데이터를 전송할때 IP + 출발지 PORT + 목적지IP + PORT를 패킷에 담아서 보낸다.
2. 그러면 받는 서버는 출발지 PORT를 보고 그 ip에 해당 PORT로 데이터를 보낸다.

### URI

- Uniform : 리소스 식별하는 통일된 방식 이다.
- Resource : 자원, URI로 식별할 수 있는 모든 것(자원이라는 것은 광범위 하다. - 어떤 파일, 어떤 데이터 등등)
- Identifier : 다른 항목과 구분하는데 필요한 정보(식별)

**URL** : Location 어떤 자원을 위치로써 찾아내는 것

**URN** : Name 어떤 자원에 이름을 부여한것 (잘 안씀)

### HTTP

- 현재 HTTP1.1을 가장많이 쓰고 HTTP2 / 3도 늘어나는 추세이다.

*크롬에서 F12를 누른후 > Network > Name에 우클릭 > 프로토콜 체크해서 보면 h2, h1.1, h3 등등을 확인할 수 있다.

### HTTP 특징

- 클라이언트 - 서버 구조
- 무상태 프로토콜 (Stateless) - 이전에 요청한 상태를 기억하지 못함
- 비연결성 - 계속 연결을 하지않고 한번 요청 한번응답 끝
    - 한계
        - 계속 TCP/IP 3way handshake를 함
        - 하지만, html,자바스크립트,이미지등등을 한번 연결해서 가져오게 최적화하는 등 한계를 많이 해결함

> 대용량 트래픽(선착순 이벤트, 예약)을 스테이스리스한 방식으로 설계하는 것이중요하다.
> 

### HTTP 메시지

- HTTP의 구조
    
    ![http구조](https://github.com/YouJedong/kyh_http_network_lecture/assets/108327853/7f616c74-ccc7-47be-8212-0bd55355e3d2)
    
    - 시작 라인 ex) GET /search?q=hello(요청) / HTTP/1.1 200 OK(응답)
    - 헤더 ex) Host: www.google.com
    - 공백라인 - 무조건 공백라인을 넣음
    - message body - 데이터를 넣음

### 시작 라인 - 응답 메세지

```html
HTTP/1.1 200 OK
```

- 상태 코드(200) : HTTP 상태 코드 - 요청 성공 및 실패를 나타냄
    - 400: 클라이언트 요청 오류 - 클라이언트 쪽에서 잘못된 요청을 보냈을때
    - 500: 서버가 장애가 있거나 오류가 있을 때

### HTTP 헤더

```html
Host: www.google.com
Content-Type: text/html;charset=UTF-8
```

- 헤더필드 - field-name “:” OWS field-value OWD *OWS는 띄어쓰기 허용
- field-name은 대소문자 구분 없음 / value는 대소문자 구분함
- HTTP 전송에 필요한 모든 부가 정보들어있음

## 4. HTTP 메서드

### HTTP API를 만들 때 생각해야할 것

1. API URI는 자원(resource) 식별 관점에서!!!
    - ex) 회원 조회라면 회원이라는 자원에 초점을 두자 /member
2. 계층 구조상 상위를 컬렉션으로 보고 복수단어를 사용 권장(member → memvers)
3. 조회/등록/수정/삭제 를 만들 때 자원 식별관점으로 설계하면 어떻게 구분할까?
    - 바로 HTTP 메서드를 이용

### HTTP 기본 메서드 종류

- GET: 리소스 조회 → 데이터를 줘
- POST: 요청 데이터 처리, 주로 등록에 사용 → 내가 데이터를 줄테니 등록해줘
- PUT: 리소스를 대체, 해당 리소스가 없으면 생성 → 데이터를 줄테니 이 리소스로 대체해줘(폴더안에 넣기/덮어쓰기)
- PATCH: 리소스 부분 변경 → update
- DELETE: 리소스 삭제 → delete

### GET

- 전달할 데이터는 query parameter를 통해 전달
- 바디에 담아서 전달할 수 있지만 지원하지 않는 곳이 많아서 권장 x

### POST

- 메시지 바디를 통해 서버로 요청 데이터 전달
- 주로 전달된 데이터로 신규 데이터 등록, 프로세스 처리에 사용
- 요청 데이터를 처리한다
    - HTML form에 입력된 정보들을 처리한다.
    - 게시글, 댓글 등의 데이터를 처리한다.
    - 생성 및 변경을 넘어서 프로세스 처리할 경우(주문 시 결제완료 → 배달시작 → 배달완료)
- 다른 메서드로 처리하기 애매한 경우

> POST는 모든 요청을 처리할 수 있지만 조회는 보통 GET을 쓰고 조회를 하긴하지만 바디에 값을 넣어서 보내야 할때는 POST를 써라
> 

### PUT

- 리소스를 **완전히** 대체
    - 어떤 요청을 했을 때 해당 데이터가 없으면 생성, 해당 데이터가 있으면 대체 결론은 **해당 데이터를 덮어버림**
- *클라이언트가 리소스를 알고 식별하고 있다
    - ex) /member/100 이라는 특정 데이터에 대해 알고 있으면서 요청을 한다.
    - POST는 /member 이런 형식으로 요청을 한다는 점에서 차이가 있다.

> PUT은 수정하는 용도가 아니고 덮어쓰는 용도다.
> 

### HTTP 메서드의 속성

<p align="center" style="color:gray">
<img width="1036" alt="http 메서드 속성" src="https://github.com/YouJedong/kyh_http_network_lecture/assets/108327853/664ba287-0a82-48c7-a406-637f62b6b7b9">
김영한 강사님 HTTP메서스의 속성 강의 참조
</p> 

- 안전(Safe)
    - 호출해도 해당 리소스를 변경하지 않는다.
- 멱등(Idenpotent)
    - 한 번 호출하든 100번 호출하든 결과가 똑같음
    - PUT은 같은 요청을 여러번 해도 최종 결과는 같다.
    - 멱등하다면 자동 복구 메커니즘(클라이언트가 어떤 오류로 다시 같은 요청하는 것)을 사용할 수 있다.
    - 같은 요청을 여러번 하다가 다른 곳에서 해당 리소스를 변경했다면 결과가 다르지 않는가?
        - 멱등은 외부 요인으로 인한 리소스 변경까지는 고려하지 않음
- 캐시 가능
    - 응답 결과 리소스를 캐시해서 사용해도 되는가?
    - 실제 GET, HEAD 정도만 캐시로 사용 가능
## 5. HTTP 메서드 활용

- 정적 데이터 조회
    - ex) GET /static/star.jpg
    - 서버는 이미지, 정적인 데이터를 준다.
- 동적 데이터 조회
    - 쿼리 파라미터 사용 ex) /search?q=hello
- HTML Form 데이터 전송
    - html의 form태그를 통해 데이터를 전송하면 요청 헤더에 Content-Type과 body에 아래 같은 형식으로 들어감
        
        ```html
        <form action="/save" method="post">
        	<input type="text" name="username"/>
        </form>
        
        ---------------------------------------
        POST /save HTTP/1.1
        Host: localhost:8080
        Content-Type: application/x-www-from-urlencoded
        
        username=kim // 메세지 바디에 쿼리 파라미터 같은 형식으로 들어감 
        ```
        
    - form을 get으로도 보낼 수 있지만 from안에 데이터를 URL경로에 넣어서 전송을 한다. → 쓰지 말아야 한다.
    
    ***multipart/form-data**
    
    ```html
    <form action="/save" method="post" enctype="multipart/form-data">
    	<input type="text" name="username"/>
    	<input type="file" name="file1">
    </form>
    
    ---------------------------------------------------
    POST /save HTTP/1.1
    Host: localhost:8080
    **Content-Type:multipart/form-data; boundart=---XXX // 여러 컨텐츠 타입으로 보내짐**
    Content-Length: 10485
    
    ---XXX
    Content-Disposition: form-data; name="username"
    
    kem
    ---XXX
    Content-Disposition: form-data; name="file1"; filename="intro.png"
    
    109238fjsdofijsdofijoiadfjoadifj....
    ---XXX--
    
    ****
    ```
    
    - form태그는 GET,POST만 지원됨
- HTTP API 데이터 전송
    
    ```html
    POST /members HTTP/1.1
    Content-Type: application/json
    
    {
    	"username": "name"
    }
    ```
    
    - 서버 to 서버 / 앱클라이언트 / 웹클라이언트(Ajax, React, VueJs)에서 사용

### API 설계

- POST 기반 설계
    - ex) 회원 관리
    - 클라이언트는 회원을 등록할 때 해당 리소스의 URI를 모른다. (회원 등록 시 서버가 리소스의 URI를 생성해준다 100번 회원 101번 회원.. 등등)
    - 따라서 서버가 리소스를 생성하고 관리하는 형태를 **컬렉션(Collection)**이라고 한다.
- PUT 기반 설계
    - ex) 파일 관리 시스템
    - 클라리언트가 리소스의 URI를 알고있어야 한다. (파일을 등록할때 파일의 이름으로 등록을 요청한다. start.jpg, blrablra.png)
    - 따라서 클라이언트가 리소스를 관리하고 URI를 알고 있다 이런 형태를 스토어(Store)라고 한다.

### HTML FORM 사용

- http form을 사용하게 되면 get,post만 사용해야한다.(물론 Ajax를 사용한다면 다른 메서드도 가능)
- 따라서 uri 규칙을 정해야 한다.
    - ex) 
    회원등록 폼(회원 등록 화면을 내려주는 uri) - memeber/new → GET
    회원등록(실제 회원 등록하는 uri) - member/new → POST or member → POST
    *회원 등록 시 uri를 전자처럼 사용하는걸 **선호**
- GET,POST만 지원하기 때문에 컨트롤 URI를 사용해야한다.
*컨트롤 URI : uri에 동사형태의 경로를 사용하는 것 ex) member/{id}/delete

### 참고하면 좋은 URI 설계 개념 정리

- 문서(document)
    - 단일 개념(파일 하나, 객체 인스턴스, 데이터 베이스 row)
    - ex) member/100, /files/star.jpg
- 컬렉션(collection)
    - 서버가 관리하는 리소스 디렉터리
    - 서버가 리소스의 URI를 생성, 관리
- 스토어(store)
    - 클라이언트가 관리하는 자원 저장소
    - 클라이언트가 리소스의 URI를 알고 정리
- 컨트롤러, 컨트롤 URI
    - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
    - 동사를 직접 사용

*참고 사이트 (https://restfulapi.net/resource-naming)