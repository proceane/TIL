# @Value

## Definition
`@Value`는 Spring이 관리하는 Bean의 필드에 값을 주입하는 데 사용할 수 있으며 필드 또는 생성자/메서드 매개변수 수준에서 적용할 수 있다.([출처](https://www.baeldung.com/spring-value-annotation))

`@Value`를 사용하기 위해서는 프로퍼티 파일이 있어야한다.  
.properties나 .yml로 프로퍼티 파일을 생성한다.

## Usage
다음과 같은 방식으로 사용한다.
```java
@Value("123")
private String value;
```
위와 같이 사용하면 `value`변수에는 `"123"`이라는 문자열이 값으로 적용된다.

프로퍼티 내부에 있는 값을 가져오려면 아래와 같이 사용한다.
```java
// spring.key=springKey in .properties 
@Value("${spring.key}")
private String springKey;
```
`springKey`변수의 값은 프로퍼티 파일에 설정된대로 `"springKey"`라는 문자열이 된다.
