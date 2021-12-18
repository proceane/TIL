# ORM

## Definition
객체 관계 매핑(Object-relational mapping; ORM)은 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법이다.([출처](https://ko.wikipedia.org/wiki/%EA%B0%9D%EC%B2%B4_%EA%B4%80%EA%B3%84_%EB%A7%A4%ED%95%91))  

객체 지향 프로그래밍은 클래스를 사용하고, 관계형 데이터베이스느 테이블을 사용한다. 
ORM을 통해 SQL을 자동으로 생성하여 객체 모델과 관계형 모델 간의 불일치를 해결한다.([출처](https://gmlwjd9405.github.io/2019/02/01/orm.html))

## Pros and Cons
- 장점
    - 객체 지향적인 코드로 인해 비즈니스 로직에 집중할 수 있게 해줌
    - 재사용 및 유지보수의 편리성이 증가
    - DBMS 종속성이 줄어듬
- 단점
    - ORM으로 완벽하게 서비스를 구현하기 어려움
    - 프로시저가 많은 시스템에서는 ORM을 활용하기 힘듬

## Impedence Mismatch
