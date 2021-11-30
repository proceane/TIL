# AOP

## Definition
관점 지향 프로그래밍(aspect-oriented programming, AOP)은 횡단 관심사(cross-cutting concern)의 분리를 허용함으로써 모듈성을 증가시키는 것이 목적인 프로그래밍 패러다임이다.([출처](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%A0%90_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D))  

간단하게 설명하면 흩어진 Aspect들을 모아서 모듈화하는 기법이라고 한다.([출처](https://velog.io/@max9106/Spring-AOP%EB%9E%80-93k5zjsm95))  

개발을 하면서 발생하는 공통적인 기능들을 하나로 묶어서 어디서든 사용하기 위해 적용한다.  

AOP는 핵심기능과 공통기능을 분리한다.([출처](https://hongku.tistory.com/114))

## Term
[출처](https://hongku.tistory.com/114)
- Aspect : 핵심 기능
- Advice : Aspect의 기능 자체
- JointPoint : Advice를 적용해야되는 부분(스프링에서 메소드)
- PointCut : JointPoint의 부분으로 실제로 Advice가 적용되는 부분
- Weaving : Advice를 핵심 기능에 적용하는 행위
