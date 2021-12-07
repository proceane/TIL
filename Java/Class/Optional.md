# Optional

## Definition
Optional<T> 클래스는 Integer나 Double 클래스처럼 'T'타입의 객체를 포장해 주는 래퍼 클래스(Wrapper class)다.(

Optional 객체를 사용하면 NullPointerException으로 인한 프로세스 중지 상태를 쉽게 피할수 있다.  
복잡한 조건문 없이도 Null로 인한 예외를 회피할 수 있다.

## Method
Optional 객체는 다음과 같이 생성할 수 있다.
```java
Optional<String> opt = Optional.ofNullable("Optional Class");
```
만약 `of()`를 사용했을 때 null을 삽입하였다면 NullPointerException이 발생한다.  
그래서 `ofNullable()`을 사용하여 null일 때 빈 Optional 객체를 반환하는 것이 좋다.

Optional 객체에 저장된 클래스에 접근하려면 `get()` 메소드를 사용하면 된다.  
하지만 값이 null일 때 `get()`을 실행하면 NullPointerException이 발생한다.  
그래서 `ifPresent()`로 null여부를 체크한 후 `get()`을 사용하는 것이 좋다.
```java
Optional<String> opt = Optional.ofNullable("Optional Class");
if(opt.isPresent()) {
  System.out.println(opt.get());
}
```

다음과 같은 메소드를 사용하여 null 대신에 반환할 값을 지정할 수 있습니다.
- `orElse()` : 값이 있으면 해당 값을, 없으면 파라미터에 지정된 값을 반환
- `orElseGet()` : 값이 있으면 해당 값을, 없으면 인수로 전달된 람다식의 결과값을 반환
- `orElseThrow()` : 값이 있으면 해당 값을, 없으면 인수로 전달된 예외를 발생

그 외에도 다음과 같은 메소드가 존재한다.  
`empty()` : 빈 Optional 객체를 생성

## Reference
[Optional 클래스](http://www.tcpschool.com/java/java_stream_optional)
