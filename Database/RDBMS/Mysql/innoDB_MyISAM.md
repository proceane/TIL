# InnoDB, MyISAM

InnoDB와 MyISAM은 MySQL에서 사용되는 대표적인 스토리지 엔진이다.

### Definition
MyISAM은 기본 스토리지 시스템이었지만, 현재는 기본 스토리지가 InnoDB로 교체되었다.  
InnoDB는 사용자 데이터를 보호하고 내결함성을 제공하는 커밋, 롤백 및 크래시 복구 기능과 같은 기능을 갖춘 MySQL용 트랜잭션 안전(ACID 호환) 스토리지 엔진이다.  

### Description
InnoDB와 MyISAM 비교

1. 스토리지 유형
2. 트랜잭션
3. ACID
4. 성능
5. 외래 키 지원
6. 잠금
7. 캐싱 및 인덱싱

### 여담
InnoDB의 인덱스 제한 값은 변경이 되는것 같은데 MyISAM은 변경이 가능한가?  
공식 문서에는 변경이 가능하다고 나와있지만 방법을 찾지 못해서 슬프다

### Reference
https://hevodata.com/learn/myisam-vs-innodb/
https://blog.devart.com/myisam-vs-innodb.html
https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html
