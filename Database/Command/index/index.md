# Index

## Definition

[Wikipedia](https://en.wikipedia.org/wiki/Database_index)에 명시된 데이터베이스 인덱스의 정의는 다음과 같다.
> A database index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure.

데이터베이스 인덱스는 인덱스 데이터 구조를 유지하기 위해 추가적인 쓰기 및 저장 공간을 사용하여 데이터베이스 테이블에 대한 데이터 검색 작업의 속도를 향상시키는 데이터 구조라고 한다.

## SQL Statement

* Create
```SQL
CREATE INDEX 인덱스명 ON 테이블명(컬럼1, 컬럼2, 컬럼3...);
```
* Add
```SQL
ALTER TABLE 테이블명 ADD INDEX 인덱스명 (컬럼1, 컬럼2..);
```
* Drop
```SQL
DROP INDEX 인덱스명 ON 테이블명;
```
* SELECT
```SQL
SHOW INDEX FROM 테이블명;
```
## Usage
인덱스는 다음과 같은 상황에서 사용한다.
1. where절에 자주 사용되는 컬럼
2. join이 잦은 컬럼
3. 외래키가 되는 컬럼

다음과 같은 상황에서는 사용을 피해야 한다.
1. INSERT, UPDATE, DELETE가 많이 일어나는 컬럼
2. 중복이 많은 컬럼(ex. 성별)

## Reference
https://velog.io/@gillog/SQL-Index%EC%9D%B8%EB%8D%B1%EC%8A%A4  
https://lalwr.blogspot.com/2016/02/db-index.html

## 여담
오늘(2021.11.10) 인덱스때문에 큰 충격을 받았었다.  
82만 데이터 풀스캔 4회 로직을 1초도 안되는 시간으로 처리해줬기 때문이다.  
물론 그 테이블은 단순한 로그 저장용 테이블이라 데이터의 수정이나 삭제가 일어나지 않았기에 인덱스의 효과가 더 잘 나타났던 것 같다.  
이런게 개발의 재미라고 생각한다.