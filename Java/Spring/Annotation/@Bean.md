# @Bean

## Definition
Bean 자체는 Spring IoC 컨테이너가 관리하는 자바 객체를 부르는 말이라고 한다.([출처](http://melonicedlatte.com/2021/07/11/232800.html)) 

`@Bean`은 다음과 같이 정의하고 있다.  
[Document](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html)

> Indicates that a method produces a bean to be managed by the Spring container.  

메소드가 스프링 컨테이너에 의해 관리될 bean을 생성한다는 것을 나타낸다.

그래서 자바 객체로써의 Bean과 어노테이션 @Bean은 다른 의미라고 볼 수 있다.

어떤 메소드를 Bean으로 등록하고싶을때, 해당 메소드에 `@Bean`을 붙이면 스프링 컨테이너에 의해 관리되는 Bean이 된다.

[@Configuration](https://github.com/proceane/TIL/blob/master/Java/Spring/Annotation/%40Configuration.md)과 함께 사용하거나, `@Component`와 함께 사용하기도 한다.

개발자가 건드릴 수 없는 외부 라이브러리를 빈으로 사용하려 할 때 `@Bean`을 붙여서 사용할수 있다.([출처](https://velog.io/@composite/Spring-Component-Bean-%EC%95%8C%EA%B3%A0-%EC%93%B0%EA%B8%B0))

## Example
```java
@Bean
public MyBean myBean() {
  // instantiate and configure MyBean obj
  return obj;
}
```
위와 같이 사용하면 myBean이라는 이름의 Bean이 생성된다.

```java
@Bean({"b1", "b2"}) // bean available as 'b1' and 'b2', but not 'myBean'
public MyBean myBean() {
  // instantiate and configure MyBean obj
  return obj;
}
```
Bean의 이름을 변경할 수도 있다.  
다만 기존의 myBean이라는 이름은 사용못하는 것 같다.

```java
@Bean
@Profile("production")
@Scope("prototype")
public MyBean myBean() {
  // instantiate and configure MyBean obj
  return obj;
}
```
`@Profile`과 `@Scope`로 해당 빈의 사용 범위를 정할 수 있다.  

`@Profile`은 properties나 yml에 선언된 spring.profiles.active값에 따라 해당 Bean을 사용할지 말지 설정할 수 있고,
`@Scope`는 Bean의 범위를 싱글톤에서 다른 지정된 범위로 변경할 수 있다.

```java
@Configuration
public class AppConfig {

  @Bean
  public FooService fooService() {
    return new FooService(fooRepository());
  }

  @Bean
  public FooRepository fooRepository() {
    return new JdbcFooRepository(dataSource());
  }

  // ...
 }
```
`@Configuration`이 선언된 class안에 `@Bean`을 사용하면 해당 클래스에 같이 선언된 Bean을 직접 사용할 수 있다.  
만약 `@Configuration`이 선언되지 않았다면 다른 Bean을 사용할 수 없게 된다.  
위의 코드에서 `@Configuration`이 없다면 `new FooService(fooRepository())`부분에 컴파일 오류가 발생한다.

```java
@Component
public class Calculator {
  public int sum(int a, int b) {
    return a+b;
  }

  @Bean
  public MyBean myBean() {
    return new MyBean();
  }
}
```
`@Component`를 사용하면 `@Bean`의 Lite Mode가 된다.
`@Configuration`과 다르게 @Bean간 참조가 안된다고 한다.
대신 한 @Bean 메소드가 다른 @Bean 메소드를 참조하려 할때 Java 표준 방식으로 참조한다.

### 여담
Spring 사용할 때 많이 썼지만, 쓰면서도 어려움을 많이 느꼈었다.
이렇게 정리해두니 조금은 더 알게된 듯 싶다. 