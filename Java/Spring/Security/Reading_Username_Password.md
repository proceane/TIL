# Reading Username/Password
Spring Security는 HttpServletRequest에서 사용자 이름과 비밀번호를 읽기 위해 다음과 같은 기본 제공 매커니즘을 제공함

### Form Login
[https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html)

Spring Security는 html 형식을 통해 제공되는 사용자 이름과 비밀번호에 대한 지원을 제공합니다.

이 섹션에서는 Spring Security 내에서 양식 기반 인증이 작동하는 방식에 대해 자세히 설명합니다.

스프링 시큐리티 내에서 폼 기반 로그인이 작동하는 방식을 살펴봅니다.

먼저 사용자가 로그인 폼으로 리디렉션 되는 방법을 확인해보면
1. 먼저 사용자가 권한이 부여되지 않은 리소스 url에 인증되지 않은 요청을 함
2. Spring Security의 FilterSecurityInterceptor는 AccessDeniedException을 던져 인증되지 않은 요청이 거부되었음을 나타냄
3. 사용자가 인증되지 않았기때문에 ExceptionTranslationFilter는 인증 시작을 시작하고 구성된 AuthenticationEntryPoint가 있는 로그인 페이지로 리디렉션을 보냄. 대부분의 경우 LoginUrlAuthenticationEntryPoint의 인스턴스임
4. 브라우저는 리디렉션된 로그인 페이지를 요청함
5. 응용 프로그램 내에서 로그인 페이지를 렌더링

> 인증이 필요한 url을 인증없이 요청 → FilterSecurityInterceptor가 AccessDeniedException을 발생시킴 → AuthenticationEntryPoint에 의해 로그인 페이지 url로 리디렉션 → 브라우저에 의해 로그인 페이지 url이 실행 → 컨트롤러에서 로그인 페이지 url 매핑된 곳을 찾음 → 로그인 뷰 리턴 이런 순서?
> 

폼에서 사용자 이름과 암호가 제출되면 UsernamePasswordAuthenticationFilter가 사용자 이름과 암호를 인증합니다.
UsernamePasswordAuthenticationFilter가 사용자 이름과 암호를 인증합니다.
UsernamePasswordAuthenticationFilter는 AbstractAuthenticationProcessFilter를 확장하므로 로직이 유사합니다.
1. 사용자가 사용자 이름과 암호를 제출하면 UsernamePasswordAuthenticationFilter는 HttpServletRequest에서 사용자 이름과 암호를 추출하여 인증 유형인 UsernamePasswordAuthenticationToken을 만듬
2. 다음으로 UsernamePasswordAuthenticationToken이 인증을 위해 AuthenticationManager로 전달됨. AuthenticationManager가 어떻게 생겼는지에 대한 세부사항은 사용자 정보가 저장되는 방식에 따라 다름
3. 인증 실패시 SecurityContextHolder가 지워짐, RememberMeServices.loginFail이 호출됨(구성되지 않으면 작동안함), AuthenticationFailureHandler가 호출됨
4. 인증이 성공하면 SessionAuthenticationStrategy는 새 로그인에 대한 알림을 받음. Authentication은 SecurityContextHolder에서 설정됨. RememberMeServices.loginSuccess가 호출됨(구성되지않으면 작동 안함). ApplicationEventPublisher는 InteractiveAuthenticationSuccessEvent를 게시함. AuthenticationSuccessHandler가 호출됨(일반적으로 로그인 페이지로 리디렉션할때 ExceptionTranslationFilter에 의해 저장된 요청으로 리디렉션되는 SimpleUrlAuthenticationSuccessHandler임)

Spring Security 폼 로그인은 기본적으로 활성화되어있습니다.
그러나 서블릿 기반 구성이 제공되는 즉시 양식 기반 로그인이 명시적으로 제공되어야 합니다.

기본 html 폼에 대한 몇가지 핵심 사항
- /login은 post 요청을 수행해야 함
- 폼에는 thymeleaf에 의해 자동으로 포함되는 csrf 토큰이 포함되어야 함
- 폼에는 username이라는 매개변수에 사용자이름을 지정해야 함
- 폼에는 password라는 매개변수에 비밀번호를 지정해야 함
- HTTP 매개변수 오류가 발견되면 사용자가 유효한 사용자 이름, 비밀번호를 제공하지 않았음을 나타냄
- HTTP 매개변수 logout이 발견되면 사용자가 성공적으로 로그아웃했음을 나타냄

많은 사용자는 로그인 페이지를 사용자 정의하는 것 이상을 필요로 하지 않습니다.
그러나 필요한 경우 위의 모든 항목을 추가 구성으로 사용자 지정할 수 있습니다.

Spring MVC를 사용하는 경우 GET /login을 우리가 생성한 로그인 템플릿에 매핑하는 컨트롤러가 필요합니다.

### Basic Authentication
[https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html)

이 섹션에서는 Spring Security가 서블릿 기반 애플리케이션에 대한 기본 HTTP 인증 지원을 제공하는 방법에 대해 자세히 설명합니다.

스프링 시큐리티 내에서 HTTP 기본 인증이 어떻게 작동하는지 살펴봅니다.
먼저 WWW-Authenticate 헤더가 인증되지 않은 클라이언트로 다시 전송되는 것을 볼 수 있습니다.
1. 먼저 사용자가 권한이 부여되지 않은 리소스에 인증되지 않은 요청을 함
2. Spring Security의 FilterSecurityInterceptor는 AccessDeniedException을 던져 인증되지 않은 요청이 거부되었음을 나타냄
3. 사용자가 인증되지 않았으므로 ExceptionTranslationFilter가 인증 시작을 시작함. 구성된 AuthenticationEntryPoint는 WWW-Authenticate 헤더를 보내는 BasicAuthenticationEntryPoint의 인스턴스임. RequestCache는 일반적으로 클라이언트가 원래 요청한 요청을 재생할 수 있으므로 요청을 저장하지 않는 NullRequestCache입니다.

클라이언트가 WWW-Authenticate 헤더를 수신하면 사용자 이름과 암호로 재시도해야 함을 알고 있습니다.

처리중인 사용자 흐름과 비밀번호의 흐름
1. 사용자가 이름과 암호를 제출하면 BasicAuthenticationFilter는 HttpServletRequest에서 사용자 이름과 암호를 추출하여 인증 유형인 UsernamePasswordAuthenticationToken을 만듬
2. 다음으로 UsernamePasswordAuthenticationToken이 인증을 위해 AuthenticationManager로 전달됨. AuthenticationManager가 어떻게 생겼는지에 대한 세부 사항은 사용자 정보가 저장되는 방식에 따라 다릅니다.
3. 인증 실패시 SecurityContextHolder가 지워집니다. RememberMeServices.loginFail이 호출됩니다. 저를 기억하십시오가 구성되지 않은 경우 이것은 작동하지 않습니다. AuthenticationEntryPoint는 WWW-Authenticate가 다시 전송되도록 트리거하기 위해 호출됩니다.
4. 인증이 성공하면 인증은 SecurityContextHolder에서 설정됩니다. RememberMeServices.loginSuccess가 호출됩니다. 저를 기억하십시오가 구성되지 않은 경우 이것은 작동하지 않습니다. BasicAuthenticationFilter는 나머지 응용 프로그램 논리를 계속하기 위해 FilterChain.doFilter(request,response)를 호출합니다.

Spring Security의 HTTP 기본 인증 지원은 기본적으로 활성화되어 있습니다. 그러나 서블릿 기반 구성이 제공되는 즉시 HTTP 기본이 명시적으로 제공되어야 합니다.

> form은 아이디랑 패스워드 받아서 인증할 수 있게하는거고 basic은 헤더에 www-authenticate체크?해서 인증할 수 있게 해주는거라고 생각하면 될듯
>
