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