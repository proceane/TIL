# Architecture
[Architecture](https://docs.spring.io/spring-security/reference/servlet/architecture.html)

## Filter
스프링 시큐리티 서블릿은 서블릿 필터를 기반으로 하기때문에 필터를 먼저 살펴보는 것이 도움이 됩니다.  
<img src="https://docs.spring.io/spring-security/reference/_images/servlet/architecture/filterchain.png">  

클라이언트는 애플리케이션에 요청을 보내고, 컨테이너는 요청 uri의 경로를 기반으로 HttpServletRequest가 수행하는 필터와 서블릿이 포함된 FilterChain을 생성합니다.  
Spring MVC 애플리케이션에서 서블릿은 DispatcherServlet의 인스턴스입니다.  
Servlet의 대부분은 단일 HttpServletRequest와 HttpServletResponse를 다룹니다.  
그러나 다음과 같은 용도로 하나 이상의 필터를 사용할 수 있습니다.  
- 다운스트림 필터나 서블릿의 호출을 방지합니다. 이 경우 필터는 HttpServletResponse를 쓰게 될 것입니다.
- 다운스트림 필터나 서블릿에 의해 사용된 HttpServletRequest나 HttpServletResponse를 수정합니다. 

## DelegatingFilterProxy
스프링은 서블릿 컨테이너의 라이프사이클과 스프링의 ApplicationContext 사이를 연결하는 DelegatingFilterProxy라는 필터 구현체를 제공합니다.  
서블릿 컨테이너는 필터를 등록하고 사용할 수는 있지만 스프링 빈으로는 적용되지 않습니다.  
DelegatingFilterProxy는 표준 서블릿 컨테이너 매커니즘을 통해 등록할 수는 있지만 모든 작업을 Filter를 구현하는 스프링 빈에 위임합니다.  

DelegatingFilterProxy는 ApplicationContext로부터 Bean Filter를 찾고 그 필터를 호출합니다.  

DelegatingFilterProxy의 또다른 장점은 필터 빈 인스턴스를 찾는 것을 지연시킬 수 있습니다.  
이것은 컨테이너가 시작하기 전에 필터 인스턴스를 등록할 필요가 있기때문에 중요합니다.  
그러나 스프링은 일반적으로 ContextLoaderListener를 사용하여 Filter 인스턴스를 등록해야 할때까지 수행되지 않는 스프링 빈을 로드합니다.  

## FilterChainProxy  
스프링 시큐리티의 서블릿은 FilterChainProxy를 지원합니다.  
FilterChainProxy는 SecurityFilterChain을 통해 많은 필터 인스턴스에 위임할 수 있게 하는 Spring Security에서 제공하는 특수 필터입니다.  
FilterChainProxy가 빈이기 때문에 그것은 DelegatingFilterProxy로 감싸져 있습니다.  

### 1차 요약
1. 서블릿은 필터를 통해 요청과 응답을 제어할 수 있다.  
2. 필터는 스프링 빈이 아니기때문에 스프링 자체에서는 필터를 관리할 수가 없다.
3. 그래서 DelegatingFilterProxy를 통해 필터를 빈으로 등록하여 스프링에서 필터를 사용할 수 있다.  
4. DelegatingFilterProxy는 필터를 직접 실행하거나 그런건 아니고 ApplicationContext에서 빈으로 등록된 필터를 찾고 그 필터를 호출하는 방식으로 사용한다.  
5. DelegatingFilterProxy 내부에는 FilterChainProxy가 있는데, FilterChainProxy에는 스프링 시큐리티에서 제공하는 SecurityFilterChain이 있다.  
- FilterChainProxy는 빈으로 관리됨. 
- 흐름은 DelegatingFilterProxy -> FilterChainProxy -> 만약 시큐리티 설정을 했다면 SecurityFilterChain

그리고 짚고 넘어가야 하는것  
Filter는 서블릿의 기능이고, DelegatingFilterProxy는 스프링 빈, FilterChainProxy는 스프링 시큐리티의 기능이다.  
그리고 DelegatingFilterProxy와 FilterChainProxy는 둘다 필터로서 기능한다.

### SecurityFilterChain
SecurityFilterChain은 FilterChainProxy에서 요청에 대해 호출해야 하는 보안 필터를 결정하는 데 사용됩니다.  

SecurityFilterChain은 Bean이지만 DelegatingFilterProxy 대신 FilterChainProxy에 등록됩니다.  
FilterChainProxy는 서블릿 컨테이너 또는 DelegatingFilterProxy에 직접 등록할 때 많은 이점을 제공합니다.
첫째, 스프링 시큐리티의 모든 서블릿 지원을 위한 시작점을 제공합니다.  
둘째, FilterChainProxy는 스프링 시큐리티의 핵심이기때문에 필수 작업들을 수행할 수 있습니다.  
예를 들면 메모리 누수를 피하기 위해 SecurityContext를 지우는데, 그것은 스프링 시큐리티의 HttpFirewall을 적용하여 특정 유형의 공격으로부터 애플리케이션을 보호해줍니다.  

또한 SecurityFilterChain을 호출해야하는 시기를 결정할 때 더 많은 유연성을 제공합니다.  
FilterChainProxy는 RequestMatcher를 활용하여 HttpServletRequest의 모든 항목을 기반으로 호출을 결정할 수 있습니다.  

실제로 FilterChainProxy를 사용하여 어떤 SecurityFilterChain을 사용해야 하는지 결정할 수 있습니다.  
이를 통해 애플리케이션의 여러 부분에 대해 완전히 별도의 설정을 제공할 수 있습니다.  

예를 들면 uri이나 헤더나 쿠키로 시큐리티 설정을 구분할 수가 있음

SecurityFilterChain 부분은 다시 보기로