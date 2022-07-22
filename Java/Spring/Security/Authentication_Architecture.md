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