# RESTful API

### Definition  
REST(REpresentation State Transfer)는 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍쳐의 한 형식([출처](https://ko.wikipedia.org/wiki/REST))  

간단히 말하면 URI와 HTTP 메소드를 이용해 객체화된 서비스에 접근하는 것([출처](https://wooaoe.tistory.com/51))  

웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있음([출처](https://velog.io/@heumheum2/%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91-%EC%A4%80%EB%B9%84%ED%95%98%EA%B8%B0-2))  

### REST 6가지 원칙  
- 인터페이스 일관성(Uniform Interface) : 일관적인 인터페이스로 분리되어야 함  
- 무상태(Stateless) : 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안됨  
- 캐시 처리 기능(Cacheable) : WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 함  
- 계층화(Layered System) : 클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지 알 수 없음  
중간 서버는 로드 밸런싱 기능이나 공유 캐시 기능을 사용함으로써 시스템 규모 확장성을 향상시키는데 유용  
- Code on Demand : 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있음  

### RESTful하게 API를 디자인한다는 것은?  
[출처](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Development_common_sense)  
1. 리소스와 행위를 명시적이고 직관적으로 분리  
  - 리소스는 URI로 표현, 리소스가 가리키는 것은 명사로 표현  
  - 행위는 Http 메소드로 표현  
2. Message는 Header와 Body를 명확하게 분리해서 사용  
  - Entity는 Body  
  - API 버전 정보, MIME 타입 등은 Header에 담음  
  - header와 body는 http header와 http body로 나눌 수 있고, http body에 들어가는 json 구조로 분리할 수도 있음  
3. API 버전 관리  
  - 환경은 항상 변하기때문에 API의 Signature가 변경될 수 있음에 유의  
  - 특정 API를 변경할 때는 반드시 하위 호환성을 보장  
4. 서버와 클라이언트가 같은 방식을 사용해서 요청  
  - 다른 말로 하면 URI가 플랫폼 중립적이어야 함  
  
### RESTful API 설계  
[출처](https://sisparang.tistory.com/38)  
- URI는 정보의 자원을 표현(리소스명은 동사보다는 명사)  
- 자원에 대한 행위는 HTTP Method  
- 주의점  
  - 슬래시 구분자는 계층관계를 나타내는 데 사용  
  - URI 마지막 문자로 슬래시 포함하지 않음  
  - 하이픈은 URI 가독성을 높이는데 사용  
  - 밑줄은 URI에 사용하지 않음  
  - URI 경로에는 소문자가 적합  
  - 파일 확장자는 URI에 포함하지 않음  

### 장단점  
[출처](https://theheydaze.tistory.com/563)  
장점  
- 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화  
- 하이퍼미디어 API의 기본을 충실히 지키면서 범용성 보장  
- HTTP 프로토콜 표준을 최대한 활용  

단점  
- 브라우저를 통해 테스트할 일이 많다면 Header 설정이 어려워짐  
- 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재  