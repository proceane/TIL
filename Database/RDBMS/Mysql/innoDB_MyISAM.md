# InnoDB, MyISAM

InnoDB와 MyISAM은 MySQL에서 사용되는 대표적인 스토리지 엔진이다.

### Definition
MyISAM은 기본 스토리지 시스템이었지만, 현재는 기본 스토리지가 InnoDB로 교체되었다.  
InnoDB는 사용자 데이터를 보호하고 내결함성을 제공하는 커밋, 롤백 및 크래시 복구 기능과 같은 기능을 갖춘 MySQL용 트랜잭션 안전(ACID 호환) 스토리지 엔진이다.  

### Description
InnoDB와 MyISAM 비교

1. 스토리지 유형
  - MyISAM은 비트랙션 스토리지 엔진이다. 트랜잭션을 지원하지 않으며, 필요한 경우 수동으로 변경사항을 롤백해야 한다.
  - InnoDB는 트랜잭션 스토리지 엔진이다. 데이터 조작 시 트랜잭션이 완료되지 않는 경우 롤백이 자동으로 트리거됨을 의미한다.

2. ACID
  - ACID는 원자성, 일관성, 격리, 내구성을 나타낸다.
  - MyISAM은 ACID 속성을 지원하지 않고 InnoDB는 ACID 속성을 지원한다.
  - 오류 또는 시스템 장애가 발생한 경우 ACID 속성을 준수한다면 트랜잭션이 완료처리된다.
  
3. 성능
  - InnoDB는 트랜잭션 속성, 즉 롤백과 커밋을 지원하며 쓰기 속도가 더 빠르고 대용량 데이터 성능이 우수하다.
  - MyISAM은 트랜잭션 속성을 지원하지 않으며, 빠른 읽기를 제공한다. 대용량 데이터 성능은 InnoDB에 비해 떨어진다.

4. 외래 키 지원
  - 외래 키는 다른 테이블의 기본 키를 참조하는 속성 집합이다.
  - MyISAM을 사용하면 외래 키 제약 조건을 추가할 수 없지만, InnoDB는 외래 키 옵션을 지원한다.
  
5. 잠금
  - 잠금 기능은 데이터의 유효성을 유지시킨다. 잠금 기능이 활성화 되면 두 명 이상의 사용자가 동시에 동일한 데이터를 수정할 수 없게된다.
  - MyISAM은 테이블 잠금을 사용한다. 테이블 잠금은 메모리를 많이 필요로 하지 않는 읽기 전용 데이터베이스에 사용할 수 있다.
  - InnoDB는 행 수준 잠금을 사용한다. 선택한 행에서 여러 세션을 지원하기 위해 행이 잠겨있다. 둘 이상의 활성 사용자가 필요한 데이터베이스에 적합하며, 크래시 복구도 우수하다.
  
6. 캐싱 및 인덱싱
  - InnoDB는 데이터와 인덱스를 모두 캐시하는 대규모 버퍼 풀을 지원한다. Full-Text 검색을 지원하지 않았으나, MySQL 5.6부터 지원한다고 한다.
  - MySQL 키 버퍼는 인덱스 전용이며, 전체 텍스트 검색을 지원한다.
  - 주요 캐싱 매커니즘은 MYI 파일을 사용하여 페이지를 캐시하는 캐시 키다.

### 여담
InnoDB의 인덱스 제한 값은 변경이 되는것 같은데 MyISAM은 변경이 가능한가?  
공식 문서에는 변경이 가능하다고 나와있지만 방법을 찾지 못해서 슬프다

### Reference
https://hevodata.com/learn/myisam-vs-innodb/
https://blog.devart.com/myisam-vs-innodb.html
https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html
https://dba.stackexchange.com/questions/1/what-are-the-main-differences-between-innodb-and-myisam
