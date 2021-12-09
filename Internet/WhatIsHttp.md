# What is Http?

## Definition
Http에 대한 정의는 다음과 같다.  
- Http는 W3상에서 정보를 주고받을 수 있는 프로토콜이다.([출처](https://ko.wikipedia.org/wiki/HTTP))  
- HTTP는 HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜이다.([출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview))  
- Communication between client computers and web servers is done by sending HTTP Requests and receiving HTTP Responses([출처](https://www.w3schools.com/whatis/whatis_http.asp))  
클라이언트 컴퓨터와 웹 서버 간의 통신은 HTTP 요청 을 보내고 HTTP 응답을 수신 하여 수행된다.

Http는 웹상에서 HTML과 같은 리소스를 주고받을 때 사용하는 프로토콜이라고 정리할 수 있다.

## HTTP Message
Http 요청 메시지는 다음과 같이 구성된다.
- HTTP 메서드 : 클라이언트가 수행하고자 하는 동작을 정의한 GET, POST 같은 동사나 OPTIONS나 HEAD와 같은 명사.일반적으로, 클라이언트는 리소스를 가져오거나(GET을 사용하여) HTML 폼의 데이터를 전송(POST를 사용하여)하려고 하지만, 다른 경우에는 다른 동작이 요구될 수도 있다.
- 리소스 경로 : 프로토콜(http://) 또는 도메인, TCP 포트
- HTTP 프로토콜의 버전
- 서버에 추가적인 정보를 전달하는 헤더들
- POST와 같은 몇 가지 메서드를 위한, 전송된 리소스를 포함하는 응답의 본문과 유사한 본문.  

Http 응답 메시지는 다음과 같이 구성된다.
- Http 프로토콜의 버전
- 요청의 성공여부, 상태코드
- OK(200), Bad Request(400)과 같은 상태 메시지
- 요청 헤더와 비슷한 Http 헤더들
- 가져온 리소스가 포함되는 본문

