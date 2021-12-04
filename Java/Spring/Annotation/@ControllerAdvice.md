# @ControllerAdvice, @RestControllerAdvice

## Definition
Controller 전역에서 발생하는 예외를 처리하는 어노테이션이다.  
`@ControllerAdvice`는 에러페이지를 리턴해야할때, `@RestControllerAdvice`는 에러 Body를 리턴해야할때 사용한다.

## Use
아래 코드는 NullPointerException이 발생했을때 "null pointer exception"이라는 문자열을 리턴해주는 코드이다
```java
@RestControllerAdvice
public Class RestControllerAdvice {

  @ExceptionHandler(NullPointerException.class)
  public String nullPointerException() {
    return "null pointer exception";
  }

}
```

따로 처리해야할 Exception이 있다면 `@ControllerAdvice`나 `@RestControllerAdvice`가 선언된 클래스 내부 메소드에 `@ExceptionHandler`을 추가해주면 된다.  