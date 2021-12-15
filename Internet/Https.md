# Https

## Definition
HTTPS(HyperText Transfer Protocol over Secure Socket Layer, HTTP over TLS, HTTP over SSL, HTTP Secure)는 월드 와이드 웹 통신 프로토콜인 HTTP의 보안이 강화된 버전이다.  
HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다. 따라서 데이터의 적절한 보호를 보장한다.([출처](https://ko.wikipedia.org/wiki/HTTPS))

데이터 전송 시 암호화되지 않아서 보안이 취약한 Http와 달리 Https에서는 데이터를 암호화하여 전송한다.

## SSL, TLS
전송 계층 보안(영어: Transport Layer Security, TLS, 과거 명칭: 보안 소켓 레이어/Secure Sockets Layer, SSL)은 컴퓨터 네트워크에 통신 보안을 제공하기 위해 설계된 암호 규약이다.([출처](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EA%B3%84%EC%B8%B5_%EB%B3%B4%EC%95%88))  
보통 443 포트를 사용하며 https://로 시작한다.

TLS의 3단계 기본 절차
1. 지원 가능한 알고리즘 서로 교환
2. 키 교환, 인증
3. 대칭키 암호로 암호화하고 메시지 인증

SSL을 통한 인증 절차([출처](https://donghwi-kim.github.io/jekyll/update/2015/02/21/SSL.html))
1. 사이트에서 인증기관(CA: Certificate Authority)에 인증요청
2. 인증기관에서 검증후에 사이트의 공개키와 정보를 인증기관의 개인키로 암호화, 인증기관의 공개키는 브라우저에 제공
3. 사용자가 사이트로 접속 요청시 사이트는 인증서 전송
4. 사용자는 브라우저에 내장된 공개키로 인증서를 복호화 -> 사이트의 공개키로 대칭키를 암호화하여 전송
5. 사이트는 전송받은 암호화된 대칭키를 사이트의 개인키로 복호화 -> 사용자, 사이트 같은 대칭키 획득
6. 전송시 해당 대칭키로 암호화하여 전송


