# Authorization Architecture
[Authorization Architecture](https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html)  

## Authorities
모든 인증 구현이 GrantedAuthorities 개체 목록을 저장하는 방법을 설명합니다.  
GrantedAuthorities는 본인에게 부여된 권한을 나타냅니다.  
GrantedAuthority 객체는 AuthenticationManager에 의해 인증 객체에 삽입되고 나중에 권한 결정을 내릴 때 AuthenticationManager에서 읽습니다.  

GrandtedAuthority는 단 하나의 메소드만 있는 인터페이스입니다.  
```java
String getAuthority();
```

이 메서드를 사용하면 AuthorizationManager가 GrantedAuthority의 정확한 문자열 표현을 얻을 수 있습니다.  
표현을 문자열로 반환하면 대부분의 AuthorizationManager 및 AccessDecisionManager에서 GrantedAuthority를 쉽게 읽을 수 있습니다.  
GrantedAuthority를 문자열로 정확하게 표현할 수 없는 경우 GrantedAuthority는 복잡한 것으로 간주되고 getAuthority는 null을 반환해야 합니다.  

스프링 시큐리티는 하나의 구체적인 구현체인 SimpleGrantedAuthority를 포함합니다.  
이를 통해 사용자 지정 문자열을 GrantedAuthority로 변환할 수 있습니다.  
보안 아키텍쳐에 포함된 모든 AuthenticationProvider는 SimpleGrantedAuthority를 사용하여 인증 객체를 채웁니다.  
