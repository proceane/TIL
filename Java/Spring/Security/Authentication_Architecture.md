# Authentication Architecture
[Authentication Architecture](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html)

## ServletContextHolder
스프링 시큐리티 인증 모델의 핵심입니다.  
여기에는 SecurityContext가 포함됩니다.  

ServletContextHolder는 인증된 사람의 세부 정보를 저장하는 곳입니다.  
스프링 시큐리티는 SecurityContextHolder가 어떻게 채워지는지는 신경쓰지 않고 값이 포함되어 있으면 현재 인증된 사용자로 간주합니다.  

기본적으로 SecurityContextHolder는 ThreadLocal을 사용하여 이러한 세부 정보를 저장합니다.  
즉, SecurityContext가 해당 메서드에 대한 인수로 명시적으로 전달되지 않더라도 SecurityContext는 항상 동일한 스레드의 메서드에서 사용할 수 있습니다.  

일부 응용 프로그램은 스레드와 함께 작동하는 특정 방식때문에 ThreadLocal을 사용하는 데 완전히 적합하지 않습니다.  

SecurityContextHolder는 컨텍스트 저장 방법을 지정하기 위해 시작 시 전략으로 구성할 수 있습니다.  
독립 실행형 애플리케이션의 경우 `SecurityContextHolder.MODE_GLOBAL` 전략을 사용합니다.  
다른 응용 프로그램은 보안 스레드에 의해 생성된 스레드도 동일한 보안 ID를 가정하도록 할 수 있습니다.  
이는 `MODE_INHERITABLETHREADLOCAL`을 사용하여 수행됩니다.  

## SecurityContext
SecurityContext는 SecurityContextHolder에서 가져옵니다.  
SecurityContext는 인증 객체를 포함합니다.  

## Authentication  
Authentication은 스프링 시큐리티 내에서 두가지 중요한 목적을 제공합니다.  
- 사용자가 인증을 위해 제공한 자격 증명을 제공하기 위해 AuthenticationManager에 대한 입력입니다.  
이 시나리오에서 사용되는 경우 `isAuthenticated()`는 false를 반환합니다.  
- 현재 인증된 사용자를 나타냅니다.  
현재 인증은 SecurityContext에서 얻을 수 있습니다.  

Authentication에는 다음 항목이 포함됩니다.  
- principal : 사용자를 식별합니다. 사용자 이름, 암호로 인증할 때 UserDetails의 인스턴스입니다.  
- credentials : 일반적으로 암호입니다. 많은 경우 이 정보는 유출되지 않도록 사용자가 인증된 후 지워집니다.  
- authorities : Granted Authority는 사용자에게 부여된 상위 수준 권한입니다. 몇가지 예는 역할 또는 범위입니다.

## GrantedAuthority
GrantedAuthority는 사용자에게 부여된 상위 수준 권한입니다.  

GrantedAuthorites는 Authentication.getAuthorities메서드에서 얻을 수 있습니다.  
GrantedAuthorities는 보안 주체에게 부여된 권한으로, 일반적으로 ROLE_ADMIN, ROLE_USER와 같은 역할입니다.  
이러한 역할은 나중에 웹 권한 부여, 메소드 권한 부여, 도메인 객체 권한 부여를 위해 구성됩니다.  

스프링 시큐리티는 이러한 권한을 해석할 수 있으며, 이러한 권한을 예상합니다.  
username/password 기반 인증을 사용할 때 GrantedAuthorites는 일반적으로 UserDetailsService에 의해 로드됩니다.  

## AuthenticationManager
AuthenticationManager는 스프링 시큐리티의 필터가 인증을 수행하는 방법을 정의하는 API입니다.  
반환된 인증은 AuthenticationManager를 호출한 컨트롤러(스프링 시큐티리의 Filter)에 의해 SecurityContextHolder에 설정됩니다.  
스프링 시큐리티의 Filter와 통합하지 않는 경우 SecurityContextHolder를 직접 설정할 수 있으며 AuthenticationManager를 사용할 필요가 없습니다.  

## ProviderManager
ProviderManager는 가장 일반적으로 사용되는 AuthenticationManager의 구현체입니다.  

ProviderManager는 AuthenticationProvider 목록에 위임합니다.  
각 AuthenticationProvider는 인증이 성공하거나 실패해야함을 나타내거나 결정을 내릴 수 없음을 나타내어 다운스트림 AuthenticationProvier가 결정할 수 있도록 할 기회가 있습니다.  

AuthenticationProvider 중 어느것도 인증할 수 없으면 ProviderNotFoundException과 함께 인증이 실패합니다.  

AuthenticationProvider는 특정 유형의 인증을 수행하는 방법을 알고있습니다.  
예를 들어 하나의 AuthenticationProvider는 사용자 이름, 비밀번호의 유효성을 검사할 수 있는 반면, 다른 하나는 SAML assertion을 인증할 수 있습니다.  

Authentication은 여러 유형의 인증을 지원하고 단일 AuthenticationManager 빈만 노출하면서 매우 특정한 유형의 인증을 수행할 수 있습니다.  

ProviderManager를 사용하면 AuthenticationProvider가 인증을 수행할 수 없는 경우 참조되는 선택적 상위 AuthenticationManager를 구성할 수도 있습니다.  
부모는 AuthenticationManager일수도 있지만 ProviderManager의 인스턴스일수도 있습니다.  

실제로 여러 ProviderManager 인스턴스가 동일한 상위 AuthenticationManager를 공유할 수 있습니다.  
이것은 공통 인증이 있지만 다른 인증 매커니즘도 있는 여러 SecurityFilterChain 인스턴스가 있는 시나리오에서 다소 일반적입니다.  

기본적으로 ProviderManager는 성공적인 인증 요청에 의해 반환된 인증 객체에서 민감한 자격 정보를 지우려고 시도합니다.  
이것은 암호화같은 정보가 HttpSession에서 필요 이상으로 오래 유지되는 것을 방지합니다.  

## AuthenticationProvider
여러 AuthenticationProvider를 ProviderManager에 주입할 수 있습니다.  
각 AuthenticationProvider는 특정 유형의 인증을 수행합니다.  
예를 들어 DaoAuthenticationProvider는 사용자 이름, 비밀번호 기반 인증을 지원하는 반면 JwtAuthenticationProvider는 JWT 토큰 인증을 지원합니다.  