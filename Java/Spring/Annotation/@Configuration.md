# @Configuration

## Definition
[Document](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)

> Indicates that a class declares one or more @Bean methods and may be processed by the Spring container to generate bean definitions and service requests for those beans at runtime

한 클래스가 하나 이상의 `@Bean`메소드를 선언하고, 런타임 때 Spring Container에 의해 해당 Bean에 대한 정의와 서비스 요청을 생성하기 위해 처리된다. 

1개 이상의 Bean을 등록할 때 사용한다.  
개발자가 건드릴 수 없는 라이브러리를 사용할때 붙인다고 한다([출처](https://mangkyu.tistory.com/75))

[@Bean]() 어노테이션 공부 후 링크 등록필요

## Example
```java
 @Configuration
 public class AppConfig {

     @Bean
     public MyBean myBean() {
         // instantiate, configure and return bean ...
     }
 }
```
런타임때 실행할 Bean 메소드를 작성한다.

## Question
1. @Bean 메소드 여러개일때, Configuration어노테이션 없으면 어떻게 될까
2. 반대로 Configuration이 선언되어있지만 Bean이 없이 메소드가 생성되어있다면?  
-> 두 케이스 모두 스프링을 실행할때 컴파일 오류가 나지는 않지만, 메소드 인식(?)을 못하거나 초기에 실행되어야할 메소드를 실행 못하는듯
