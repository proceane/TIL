# Inversion of Control And DI
[Document](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)에 있는 내용을 번역 및 정리한 글입니다.

## What Is Inversion of Control?
IoC, 제어의 역전은 개체나 프로그램의 일부에 대한 제어를 컨테이너 또는 프레임워크로 이전하는 소프트웨어 엔지니어링의 원칙이다.

사용자 지정 코드가 라이브러리를 호출하는 기존 프로그래밍과 달리 IoC는 프레임워크가 프로그램의 흐름을 제어하고 사용자 지정 코드를 호출할 수 있도록 한다.
-> 이 부분에 대해 추가적인 설명을 덧붙이면, 사용자 지정 코드는 쉽게 말하자면 개발자가 직접 객체를 생성해서 사용한다고 이해하면 될 것 같다.[참고](https://chanhuiseok.github.io/posts/spring-4/)

IoC의 장점은 다음과 같다.
* 작업의 실행을 구현에서 분리
* 서로 다른 구현 간에 쉽게 전환할 수 있음
* 프로그램의 더 큰 모듈성
* 구성 요소를 격리하고나, 종속성을 모방하고 구성요소가 계약을 통해 통신할 수 있도록 하여 프로그램 테스트를 더욱 용이하게 함

IoC는 디자인 패턴의 전략 패턴, 서비스 로케이터 패턴, 팩토리 패턴 그리고 종속성 주입과 같은 다양한 방법을 통해 달성할 수 있다.

## What Is Dependency Injection?
DI, 종속성 주입은 IoC를 구현할 때 사용할 수 있는 패턴이다.  
개체를 다른 개체와 연결하거나, 개체를 다른 개체에 주입하는 것은 개체 자체에서 수행되지 않고 어셈블러에 의해 수행된다.  
기존 프로그래밍에서 객체 종속성을 만드는 방법은 다음과 같다.
```java
public class Store {
    private Item item;
 
    public Store() {
        item = new ItemImpl1();    
    }
}
```
위의 예에서는 Store클래스 내에서 Item 인터페이스의 구현을 인스턴스화 해야 한다.

DI를 사용한다면 다음과 같이 작성할 수 있다.
```java
public class Store {
    private Item item;
    public Store(Item item) {
        this.item = item;
    }
}
```
-> 이 내용을 이해하기 쉽게 설명을 덧붙이자면, 첫번째 코드는 Item 인터페이스의 구현이 Store클래스에 묶여있는데(종속), 아래 코드는 Store 클래스를 생성할 때 Store 밖에서 구현한 Item 클래스로 생성하면 된다.
Item이라는 객체의 제어권을 Store객체에 맡기지 않는다고 이해하면 될 듯 싶다.  
사실 정확하게 이해한것 같지는 않다.

## Spring IoC Container
IoC 컨테이너는 IoC를 구현하는 프레임워크의 일반적인 특성이다.  
Spring 프레임워크에 있는 ApplicationContext 인터페이스는 IoC 컨테이너를 나타낸다.  
Spring 컨테이너는 Bean 객체를 인스턴스화, 구성, 조합하고 수명 주기를 관리하는 역할을 한다.  

Bean을 모으기 위해 컨테이너는 XML 구성 또는 어노테이션의 형태일 수 있는 구성 메타데이터를 사용한다.  

컨테이너를 수동으로 인스턴스화하는 한 가지 방법은 다음과 같다.
```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```
위의 예에서 Item 속성을 설정하기 위해 메타데이터를 사용할 수 있다.
그리고 컨테이너는 이 메타데이터를 읽고 런타임 때 빈을 어셈블하는 데에 사용한다.