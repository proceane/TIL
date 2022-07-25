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

## Pre-Invocation Handling
스프링 시큐리티는 메소드 호출 또는 웹 요청과 같은 보안 객체에 대한 액세스를 제어하는 인터셉터를 제공합니다.  
호출이 계속되도록 허용되는지 여부에 대한 사전 호출 결정은 AccessDecisionManager에 의해 이루어집니다.  

### AuthorizationManager
AuthorizationManager는 AccessDecisionManager와 AccessDecisionVoter를 모두 대체합니다.  
AccessDecisionManager 또는 AccessDecisionVoter를 사용자 정의하는 애플리케이션은 AuthorizationManager를 사용하도록 변경하는 것이 좋습니다.  
AuthorizationManager는 AuthorizationFilter에 의해 호출되며, 최종 액세스 제어 결정을 담당합니다.  
```java
AuthorizationDecision check(Supplier<Authentication> authentication, Object secureObject);

default AuthorizationDecision verify(Supplier<Authentication> authentication, Object secureObject)
        throws AccessDeniedException {
    // ...
}
```
check 메소드는 승인 결정을 내리는 데 필요한 모든 관련 정보를 전달합니다.  
특히 보안 객체를 전달하면 실제 보안 개체 호출에 포함된 인수를 검사할 수 있습니다.  

verify는 verity를 호출한 다음 부정적인 AuthorizationDecision의 경우 AccessDeniedException을 throw합니다.  

### Delegate-based AuthorizationManager Implementations
사용자는 권한 부여의 모든 측면을 제어하기 위해 자신의 AuthorizationManager를 구현할 수 있지만 스프링 시큐리티는 개별 AuthorizationManager와 협력할 수 있는 위임 AuthorizationManager와 함께 제공합니다.  
RequestMatcherDelegatingAuthorizationManager는 요청을 가장 적절한 위임 AuthorizationManager와 매칭시킵니다. 메서드 보안을 위해 AuthorizationManagerBeforeMethodInterceptor 및 AuthorizationManagerAfterMethodInterceptor를 사용할 수 있습니다.

- AuthorityAuthorizationManager
스프링 시큐리티와 함께 제공되는 가장 일반적인 AuthorizationManager는 AuthorityAuthorizationManager입니다.  

- AuthenticatedAuthorizationManager
익명, 완전 인증 및 기억 인증 사용자를 구별하는 데 사용할 수 있습니다.  

- Custom Authorization Managers
사용자 지정 AuthorizationManager를 구현할 수도 있고 원하는 액세스 제어 논리를 여기에 넣을 수도 있습니다.  
애플리케이션에 따라 다르거나 일부 보안 관리 로직을 구현할 수도 있습니다.  
