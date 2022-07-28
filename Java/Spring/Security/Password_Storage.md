# Password Storage

### Storage Mechanisms
사용자 이름과 암호를 읽기 위해 지원되는 각 매커니즘은 지원되는 모든 저장 매커니즘을 활용할 수 있습니다.
- 메모리 저장
- JDBC를 이용하여 관계형 데이터베이스에 저장
- UserDetailsService가 있는 사용자 지정 데이터 저장소
- LDAP 인증을 통한 LDAP 스토리지

## In memory
Spring Security의 InMemoryUserDetails는 메모리에 저장된 사용자 이름/비밀번호 기반 인증에 대한 지원을 제공하기 위해 UserDetailsService를 구현합니다.

InMemoryUserDetailManager는 UserDetailsManager 인터페이스를 구현하여 UserDetails 관리를 제공합니다.
UserDetails 기반 인증은 인증을 위해 사용자 이름, 비밀번호를 허용하도록 구성된 Spring Security에서 사용됩니다.

메모리 유저 생성할때 비번 암호 쓸지말지, 뭐쓸지 설정도 할수있음
> 스프링 시작할때 메모리에 유저 정보를 담아두고 필요할 때 사용하는 경우같음. 유저 값을 담은 UserDetailsService를 빈으로 등록하는거지
> 

### JDBC
Spring Security의 JdbcDaoImpl은 JDBC를 사용하여 검색되는 사용자 이름, 비밀번호 기반 인증에 대한 지원을 제공하기 위해 UserDetailsService를 구현합니다.

JdbcUserDetailsManager는 JdbcDetailsImpl을 확장하여 UserDetailsManager 인터페이스를 통해 UserDetails 관리를 제공합니다.
- 기본 스키마
    - Spring Security는 JDBC 기반 인증을 위한 기본 쿼리를 제공함
    - 이 섹션에서는 기본 쿼리와 함께 사용되는 해당 기본 스키마를 제공함
    - 사용중인 쿼리 및 데이터베이스 언어에 대한 모든 사용자 정의와 일치하도록 스키마를 조정해야 함
    - 유저 스키마
        - JdbcDaoImpl은 사용자에 대한 암호, 계정 상태(활성화 또는 비활성화) 및 권한 목록(역할)을 로드하는 테이블이 필요함
        - 필요한 테이블은 유저네임, 패스워드, 사용여부를 가진 유저테이블과 유저네임으로 연관관계가 맺어진 권한 테이블
    - 그룹 스키마
        - 애플리케이션에서 그룹을 활용하는 경우 그룹 스키마를 제공해야 함
        - 그룹의 기본 스키마는 그룹 - 그룹권한 - 그룹 멤버 테이블
- 데이터소스 설정
    - JdbcUserDetailsManager를 구성하기 전에 DataSource를 생성해야 함
    - 데이터소스 생성하는 메소드를 빈으로 등록하면 되나봄
- JdbcUserDetailsManager 빈
    - 데이터소스를 파라미터로 받는 UserDetailsManager 생성 메소드를 빈으로 등록함
    - 메모리 때랑 UserDetails 객체 만드는건 똑같은데 JdbcUserDetailsManager에 createUser로 UserDetails 객체를 등록해주는 로직이 추가됨
    

### UserDetails
UserDetails는 UserDetailsService에서 반환됩니다.
DaoAuthenticationProvider는 UserDetails의 유효성을 검사하고 구성된 UserDetailsService에서 리턴된 UserDetails인 Principal을 가진 Authentication을 리턴합니다.

### UserDetailsService
UserDetailsService는 DaoAuthenticationProvider에서 사용자 이름과 암호로 인증하기 위해 사용자 이름, 암호 및 기타 속성을 검색하는데 사용됩니다.

Spring Security는 UserDetailsService의 인메모리, JDBC 구현을 제공합니다.
> 그 외에는 UserDetailsService 인터페이스를 받아서 구현한 클래스를 빈으로 등록하면 됨
> 

이것은 AuthenticationManagerBuilder가 채워지지않았고 AuthenticationProviderBean이 정의되지 않은 경우에만 사용됩니다.
> AuthenticationManagerBuilder랑 AuthenticationProviderBean이 설정이 안된 경우에만 UserDetailsService가 사용된다는 의미인듯. 만약 설정되어있으면 저 2개를 사용해서 UserDetails 객체를 만드는건가
> 

### PasswordEncoder
spring security의 서블릿은 PasswordEncoder와 통합하여 암호를 안전하게 저장하도록 지원합니다.
Spring Security에서 사용하는 PasswordEncoder 구현을 사용자 정의하려면 PasswordEncoder 빈을 노출하여 수행 가능합니다.

### DaoAuthenticationProvider
DaoAuthenticationProvider는 UserDetailsService 및 PasswordEncoder를 활용하여 사용자 이름과 암호를 인증하는 AuthenticationProvider 구현입니다.

> UserDetailsService에서 얘기한 AuthenticationProviderBuilder중에 이게 있는건가
> 

1. 사용자 이름과 암호 읽기 인증 필터는 UsernamePasswordAuthenticationManager에 의해 구현되는 AuthenticationManager에 전달함
2. ProviderManager는 DaoAuthenticationProvider 유형의 AuthenticationProvider를 사용하도록 구성됨
3. DaoAuthenticationProvider는 UserDetailsService에서 UserDetails를 찾음
4. DaoAuthenticationProvider는 PasswordEncoder를 사용하여 이전 단계에서 반환된 UserDetails의 암호를 확인함
5. 인증이 성공하면 UsernamePasswordAuthenticationToken 유형이고 구성된 UserDetailsService에서 반환한 UserDetails인 주체를 가짐. 궁극적으로 반환된 UsernamePasswordAuthenticationToken은 필터에 의해 SecurityContextHolder에 설정됨

> 순서는 UsernamePasswordAuthenticationToken → AuthenticationManager의 ProviderManager? → DaoAuthenticationProvider → UserDetailsService, PasswordEncoder → ProviderManager? → UsernamePasswordAuthenticationToken → Filter(무슨 필터인지는 모름) → SecurityContextHolder인듯
> 

### LDAP
LDAP는 조직에서 사용자 정보의 중앙 저장소 및 인증 서비스로 자주 사용됩니다.
또한 응용 프로그램 사용자에 대한 역할 정보를 저장하는 데 사용할 수 있습니다.

Spring Security의 LDAP 기반 인증은 Spring Security가 인증을 위해 사용자 이름, 비밀번호를 허용하도록 구성된 경우에 사용됩니다.
그러나 인증을 위해 사용자 이름, 비밀번호를 활용함에도 불구하고 바인딩 인증에서 LDAP 서버가 비밀번호를 반환하지 않아 애플리케이션이 비밀번호 유효성 검사를 수행할 수 없기때문에 UserDetailsService를 사용하여 통합되지 않습니다.

Spring security의 LDAP 공급자가 완전히 구성 가능하도록 LDAP 서버를 구성하는 방법에 대한 다양한 시나리오가 있습니다.
인증 및 역할 검색을 위해 별도의 전략 인터페이스를 사용하고 다양한 상황을 처리하도록 구성할 수 있는 기본 구현을 제공합니다.