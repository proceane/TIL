# Aggregate Root
[What's an aggregate root?](https://stackoverflow.com/questions/1958621/whats-an-aggregate-root)에 있는 내용을 정리  

## Definition
> In the context of the repository pattern, aggregate roots are the only objects your client code loads from the repository.  

리포지토리 패턴의 컨텍스트에서 애그리거트 루트는 클라이언트 코드가 리포지토리에서 로드하는 유일한 객체  

> An AGGREGATE is a cluster of associated objects that we treat as a unit for the purpose of data changes. Each AGGREGATE has a root and a boundary. The boundary defines what is inside the AGGREGATE. The root is a single, specific ENTITY contained in the AGGREGATE.  
The root is the only member of the AGGREGATE that outside objects are allowed to hold references to[.]
(출처 - Eric Evans)

애그리거트는 데이터 변경을 위해 하나의 단위로 취급하는 관련 객체의 클러스터  
각 애그리거트에는 루트와 경계가 존재  
경계는 애그리거트 내부에 있는 내용을 정의  
루트는 애그리거트에 포함된 단일한 특정 엔티티  
루트는 외부 객체가 [.]에 대한 참조를 보유할 수 있는 애그리거트의 유일한 멤버  

> Aggregate Root is the mothership entity inside the aggregate (in our case Computer), it is a common practice to have your repository only work with the entities that are Aggregate Roots, and this entity is responsible for initializing the other entities.  

애그리거트 루트는 애그리거트 내부의 거대한 엔티티  
저장소가 애그리거트 루트인 엔티티로만 작동하도록 하는 것이 일반적  
이 엔티티는 다른 엔티티의 초기화를 담당  

정리하자면 애그리거트 루트는  
1. 리포지토리에서 사용하는 유일한 객체  
2. 애그리거트의 객체 중 다른 객체에서 접근(참조?)할 수 있는 유일한 객체