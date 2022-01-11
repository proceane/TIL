# Isolation

## Definition
Isonlation Level이란 트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준을 의미([출처](https://effectivesquid.tistory.com/entry/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-Isolation-Level))  

예를 들어, 한 사용자가 어떠한 데이터를 수정하고 있는 경우 다른 사용자들의 접근을 차단함으로써 완전한 데이터만을 사용자들에게 제공하게 된다.  

## Level
[출처](https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation)  
- Read Uncommitted
  - 각 트랜잭션에서의 변경 내용이 Commit이나 Rollback 여부에 상관 없이 다른 트랜잭션에서 값을 읽을 수 있음
  - 정합성에 문제가 많은 격리 수준이기때문에 사용하지 않은 것을 권장
  - commit되지 않은 상태의 update값을 다른 트랜잭션에서 읽을 수 있음
  - Dirty Read 현상 발생
- Read Committed
  - RDB에서 대부분 사용하고 있는 격리 수준
  - Dirty Read와 같은 현상은 발생하지 않음
  - 실제 테이블 값이 아닌, Undo영역에 백업된 레코드에서 값을 가져옴
  - 첫번째 트랜잭션을 commit한 이후 아직 끝나지 않은 두번째 트랜잭션이 다시 테이블 값을 읽으면 값이 변경됨
  - 하나의 트랜잭션에서 똑같은 SELECT 쿼리를 실행했을 때 항상 같은 결과를 가져와야하는 Repeatable read의 정합성에 어긋남
- Repeatable Read
  - MySQL에서는 트랜잭션마다 트랜잭션 ID를 부여하여 트랜잭션 ID보다 작은 트랜잭션 번호에서 변경한 것만 읽게 됨
  - Undo공간에 백업해두고 실제 레코드 값을 변경
    - 백업된 데이터는 불필요하다고 판단하는 시점에 주기적으로 삭제
    - Undo에 백업된 레코드가 많아지면 MySQL 서버의 처리 성능이 떨어질 수 있음
    - 이러한 변경 방식은 MVCC(Multi Version Concurrency Control)라고 부름
    - Phantom Read : 다른 트랜잭션에서 수행한 변경 작업에 의해 레코드가 보였다가 안보였다가 하는 현상
      - 이를 방지하기 위해 쓰기 잠금을 걸어야 함
- Serializable
  - 가장 단순한 격리 수준이지만 가장 엄격한 격리 수준
  - 성능 측면에서는 동시 처리성능이 가장 낮음
  - Serializable에서는 Phantom Read가 발생하지 않지만 데이터베이스에서는 거의 사용되지 않음
