## Dependency Injection
[IoC](https://github.com/proceane/TIL/blob/master/Java/Spring/Theory/InversionOfControlAndDI.md)에 이어 [해당 문서](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)의 내용을 정리한 글입니다.

## Constructor-Based Dependency Injection
생성자 기반 의존성 주입의 경우, 컨테이너는 설정할 의존성을 나타내는 인수마다 생성자를 호출한다.
```java
@Configuration
public class AppConfig {

    @Bean
    public Item item1() {
        return new ItemImpl1();
    }

    @Bean
    public Store store() {
        return new Store(item1());
    }
}
```
[@Configuration](https://github.com/proceane/TIL/blob/master/Java/Spring/Annotation/%40Configuration.md)은 Bean 정의의 원천을 나타내며, 여러 구성 클래스에 추가할 수도 있다.

Bean을 정의하기 위해 메소드에 [@Bean](https://github.com/proceane/TIL/blob/master/Java/Spring/Annotation/%40Bean.md)을 사용한다.  
기본 싱글톤 범위를 가진 Bean의 경우 Spring은 Bean에 캐시된 인스턴스가 이미 존재하는지 확인하고 존재하지 않으면 새 인스턴스를 생성한다.  
프로토타입 범위를 사용하는 경우, 컨테이너는 각 메서드 호출에 대해 새로운 빈 인스턴스를 반환한다.

XML로도 Bean을 설정할 수 있다.
```xml
<bean id="item1" class="org.baeldung.store.ItemImpl1" /> 
<bean id="store" class="org.baeldung.store.Store"> 
    <constructor-arg type="ItemImpl1" index="0" name="item" ref="item1" /> 
</bean>
```

## Setter-Based Dependency Injection
setter 기반 의존성 주입의 경우, 컨테이너는 인수가 없는 생성자 또는 인수가 없는 정적 팩토리 메소드를 호출하여 Bean을 인스턴스화한 후 클래스의 setter 메소드를 호출한다.
```java
@Bean
public Store store() {
    Store store = new Store();
    store.setItem(item1());
    return store;
}
```
xml로도 동일하게 구성할 수 있다.
```xml
<bean id="store" class="org.baeldung.store.Store">
    <property name="item" ref="item1" />
</bean>
```
동일한 Bean에 대해 생성자 및 Setter 기반 의존성 주입 유형을 결합할 수 있다.  
Spring 문서에서는 필수 종속성에 대해서는 생성자 기반 주입을 사용하고, 선택적 종속성에 대해서는 setter 기반 주입을 사용하기를 권장한다.

## Field-Based Dependency Injection
필드 기반 DI의 경우 `@Autowired` 어노테이션을 사용하여 종속성을 주입할 수 있다.
```java
public class Store {
    @Autowired
    private Item item; 
}
```
Store 객체를 생성하는동안 Item Bean을 주입할 생성자 또는 설정자 메소드가 없으면 컨테이너는 리플렉션을 사용하여 Item을 Store에 주입한다.

이 방식은 간단하고 깔끔해 보이지만, 몇 가지 단점이 있다.
* 리플렉션을 사용하여 종속성을 주입하며, 이는 생성자나 Setter 기반 주입보다 비용이 많이 든다.
* 이 방식으로 여러 종속성을 계속 추가하는 것은 쉽다. 생성자 주입을 사용하는 경우 인수가 여러 개 있으면 클래스가 한 가지 이상을 수행한다고 생각하게 되어 단일 책임 원칙을 위반할 수 있다.
[@Autowired에 대한 자세한 내용](https://www.baeldung.com/spring-annotations-resource-inject-autowire)

## Autowiring Dependencies
Wiring을 통해 Spring 컨테이너는 정의된 Bean을 검사하여 협력하는 Bean들 간의 종속성을 자동으로 해결할 수 있다.

XML을 사용하여 Bean을 Autowired하는 4가지 방식이 있다.
* no : 기본값, Bean에 대해 autowiring이 사용되지 않고 의존성을 명시적으로 명명해야 함을 의미
* byName : 속성의 이름을 기반으로 autowiring이 이루어지므로 Spring은 설정해야 할 속성과 이름이 같은 Bean을 찾음
* byType : 속성 유형에만 기반한 byName autowiring과 유사함. 이것은 Spring이 설정할 동일한 유형의 속성을 가진 Bean을 찾을 것임을 의미. 해당 유형의 Bean이 2개 이상 있으면 프레임워크에서 예외 발생
* constructor : autowiring은 생성자 인수를 기반으로 수행됨. 즉 Spring은 생성자 인수와 동일한 유형의 Bean을 찾음

예를 들어, byType으로 Bean을 연결한다면
```java
@Bean(autowire = Autowire.BY_TYPE)
public class Store {
    
    private Item item;

    public setItem(Item item){
        this.item = item;    
    }
}
```

byType을 위해 `@Autowired`를 사용하여 Bean을 주입할 수도 있다.
```java
public class Store {
    
    @Autowired
    private Item item;
}
```

동일한 이름의 Bean이 2개 이상 있는 경우 `@Qualifier`를 사용하여 이름으로 Bean을 참조할 수 있다.
```java
public class Store {
    
    @Autowired
    @Qualifier("item1")
    private Item item;
}
```

## Lazy Initialized Beans
기본적으로, 컨테이너는 초기화 중에 모든 빈을 싱글톤으로 만들고 구성한다. 
이를 피하기 위해 Bean 설정에서 값이 true인 lazy-init 속성을 사용할 수 있다.
```xml
<bean id="item1" class="org.baeldung.store.ItemImpl1" lazy-init="true" />
```
위 설정으로 인해 item1은 시작할 때가 아닌 처음 요청될 때만 초기화된다.  
장점은 빠른 초기화 시간이지만, Bean이 요청될 때까지 구성 오류를 발견하지 못할 것이라는 단점이 있다.

## 여담
이렇게 정리하면서 조금씩 알아가는 느낌이 든다.
앞으로도 더 많이 알고싶다.