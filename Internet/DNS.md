## DNS

## Definition
DNS는 Domain Name System의 약자로, 호스트의 도메인 이름을 호스트의 네트워크 주소로 바꾸거나 그 반대의 변환을 수행할 수 있도록 하기 위해 개발된 시스템이다.([출처](https://ko.wikipedia.org/wiki/%EB%8F%84%EB%A9%94%EC%9D%B8_%EB%84%A4%EC%9E%84_%EC%8B%9C%EC%8A%A4%ED%85%9C))

111.111.111.111이라는 ip에 www.domain.com이라는 도메인이 지정되어 있을 때, 
DNS를 통해 www.domain.com라는 주소로 해당 ip에 접속할 수 있도록 해준다.

`nslookup`명령어를 사용하면 도메인에 지정된 ip주소를 알 수 있다.
```shell
> nslookup google.com
서버:    pcns.bora.net
Address:  61.41.153.2

권한 없는 응답:
이름:    google.com
Addresses:  2404:6800:4005:811::200e
          172.217.25.14
```

## Process
[DNS 동작방식 출처](https://gentlysallim.com/dns%EB%9E%80-%EB%AD%90%EA%B3%A0-%EB%84%A4%EC%9E%84%EC%84%9C%EB%B2%84%EB%9E%80-%EB%AD%94%EC%A7%80-%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC/)

간략하게 정리하면 다음과 같다.
> 브라우저 -> DNS서버 -> 브라우저 -> 호스팅 서버

1. 브라우저에 도메인 입력
2. DNS서버가 입력된 도메인에 할당된 호스팅서버의 ip를 찾음
3. DNS에서 받은 IP로 접속
4. 호스팅된 서버 접속

자세히 설명하면 아래와 같다.
1. 도메인을 입력하면 먼저 local DNS서버에서 해당 도메인을 찾는다.(참고 : [윈도우 hosts파일이란?](https://goddaehee.tistory.com/90))  
2. local DNS서버에 없으면 Root DNS 서버에서 도메인을 찾아줄 것을 요청한다.  
3. Root DNS 서버를 통해 루트 도메인(ex. .com)을 관리하는 Top-Level Domain 서버의 정보를 받는다.  
Top-Level Domain에서 도메인을 찾는다.  
4. 2단계 도메인을 찾아서 해당 도메인을 관리하는 DNS서버를 찾는다.
입력한 도메인이 domain.com이면 domain.com을 관리하는 DNS서버를 찾는다.
5. 2단계 도메인을 관리하는 DNS 서버에서 입력된 도메인에 할당된 ip주소를 찾아서 Local DNS서버에 알려준다.

## DNS Structure
[도메인 구성](https://xn--3e0bx5euxnjje69i70af08bea817g.xn--3e0b707e/jsp/resources/dns/dnsInfo.jsp)

루트도메인, 최상위 도메인(1단계), 2단계 도메인으로 구성되어있으며 2단계 도메인 아래에 서브도메인이 있는 경우도 있다.

### 여담
쉬워보이지만 어려운 느낌이다.  
그래도 DNS의 작동 원리는 이해가 된다.