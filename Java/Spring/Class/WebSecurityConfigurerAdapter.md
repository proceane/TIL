# WebSecurityConfigurerAdapter

## Definition
> Provides a convenient base class for creating a WebSecurityConfigurer instance.  

[출처](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/web/configuration/WebSecurityConfigurerAdapter.html)  
WebSecurityConfigurer 인스턴스를 생성하기 위한 편리한 기본 클래스를 제공한다.  

## Methods
`authenticationManager()`
- AuthenticationManager를 사용하기위해 가져옴

`authenticationManagerBean()`
- Bean으로 노출될 구성(AuthenticationManagerBuilder)에서 AuthenticationManager를 노출하려면 이 메서드를 재정의

`configure​(AuthenticationManagerBuilder auth)`
- AuthenticationManager()의 기본 구현에서 AuthenticationManager를 얻으려고 시도하는 데 사용

`configure​(HttpSecurity http)`
- HttpSecurity를 설정하기위한 메소드

`configure​(WebSecurity web)`
- WebSecurity를 설정하기위한 메소드

`getApplicationContext()`
- ApplicationContext를 얻는 메소드

`getHttp()`
- HttpSecurity를 ​​생성하거나 현재 인스턴스를 반환

`init​(WebSecurity web)`
- SecurityBuilder를 초기화

`setApplicationContext​(org.springframework.context.ApplicationContext context)`
- ApplicationContext를 지정

`setAuthenticationConfiguration​(AuthenticationConfiguration authenticationConfiguration)`
- AuthenticationConfiguration을 지정

`setContentNegotationStrategy(org.springframework.web.accept.ContentNegotiationStrategy contentNegotiationStrategy)`
- ContentNegotiationStrategy를 지정

`setObjectPostProcessor​(ObjectPostProcessor<java.lang.Object> objectPostProcessor)	`
- ObjectPostProcessor를 지정

`setTrustResolver​(AuthenticationTrustResolver trustResolver)`
- AuthenticationTrustResolver를 지정

`userDetailsService()`
- ApplicationContext와 상호 작용하지 않고 userDetailsServiceBean()에서 UserDetailsService를 수정하고 액세스 가능

`userDetailsServiceBean()`
- 이 메소드를 재정의하여 configure(AuthenticationManagerBuilder)에서 생성된 UserDetailsService를 빈으로 노출