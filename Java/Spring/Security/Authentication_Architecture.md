# Authentication Architecture
[Authentication Architecture](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html)

## ServletContextHolder
스프링 시큐리티 인증 모델의 핵심입니다.  
여기에는 SecurityContext가 포함됩니다.  

ServletContextHolder는 인증된 사람의 세부 정보를 저장하는 곳입니다.  
스프링 시큐리티는 SecurityContextHolder가 어떻게 채워지는지는 신경쓰지 않고 값이 포함되어 있으면 현재 인증된 사용자로 간주합니다.  
