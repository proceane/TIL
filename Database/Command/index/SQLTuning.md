# SQL Tuning
인덱스를 활용하여 SQL 구문을 작성하는 방법  
([해당 문서](https://d2.naver.com/helloworld/1155)의 내용을 정리)

## Index Structure
거의 모든 DBMS는 B-Tree 계열 인덱스를 사용하고 있다.  
MySQL의 InnoDB도 B-Tree를 사용한다.([출처](https://dev.mysql.com/doc/refman/8.0/en/innodb-physical-structure.html))  

## Index Scan
인덱스 범위 스캔은 다음과 같이 진행된다.  
첫번째 단계에서는 루트에서부터 트리를 순회하여 리프 노드에서 하위 키를 찾는다.  
두번째 단계에서는 첫번째 단계에서 찾은 키에서부터 상위 키까지 순차적으로 레코드를 읽어 처리한다.
상위 키가 현재 노드에서 발견되지 않으면, 다음 노드를 읽어 상위 키를 가진 노드까지 검색을 계속한다.  
상위 키까지 순차 검색이 끝나면 전체 범위 검색이 완료된다.

상위 키가 최대 키이면 현재 노드의 키부터 마지막 노드까지 모두 검색 결과에 포함되기 때문에 비교 연산을 할 필요가 없어져서 검색의 성능이 좋아진다.

아래 쿼리를 실행하여 생성된 테이블과 인덱스를 적용해보자.
```SQL
CREATE TABLE tbl  
(a INT NOT NULL,
b STRING,  
c BIGINT);

CREATE INDEX idx ON tbl  
(a, b);

INSERT INTO tbl VALUES  
(1, 'ZZZ', 123456),
(4, 'BBB', 123456789),
(1, 'AAA', 123'),
…
```

SELECT 쿼리는 다음과 같다.
```SQL
SELECT * FROM tbl  
WHERE a > 1 AND a < 5  
AND b < 'K'  
AND c > 10000  
ORDER BY b;
```

위 쿼리의 Where 검색조건은 3가지로 나눌 수 있다.
1. Key Range : 인덱스 스캔 범위로 활용하는 조건(`a > 1 AND a < 5`)
2. Key Filter : Key Range에 포함할 수는 없지만 인덱스 키로 처리 가능한 조건(`b < 'K'`)
3. Data Filter : 인덱스가 지정되어있지 않아서 인덱스를 사용할 수 없는 조건. 테이블에서 레코드를 읽어야 처리가능한 조건(c > 10000)

CUBRID의 쿼리 처리 과정을 다음과 같이 설명하고 있다.
1. 인덱스 스캔인 경우 먼저 Key Range와 Key Filter를 적용하여 조건에 부합하는 OID(개체 식별자) 리스트를 만든다. 이 과정은 Key Range의 시작부터 끝까지 계속된다.
2. OID를 이용해 데이터 페이지에서 해당 레코드를 읽어 Data Filter를 적용하거나 SELECT 리스트에 기술된 칼럼 값을 읽어와 결과를 저장하는 임시 페이지에 기록한다.
3. ORDER BY 절이나 GROUP BY 절이 있으면 임시 페이지에 저장된 레코드를 정렬하여 최종 결과를 생성한다.

## Use Index
옵티마이저가 인덱스를 사용하려면 WHERE 절에 범위 조건이 있어야 한다.  
만약 범위 조건이 없다면 옵티마이저는 테이블 순차 스캔을 시도할 것이다.

인덱스를 사용못하는 쿼리를 튜닝한 결과 다음과 같다.
|튜닝전|튜닝후|
|--|--|
|SELECT * FROM student WHERE grade <> 'A';|SELECT * FROM student WHERE grade > 'A';|
|SELECT name, email_addr FROM student WHERE email_addr IS NULL;|SELECT name,email_addr FROM student WHERE email_addr = '';|
|SELECT student_id FROM record WHERE substring(yymm, 1, 4) = '1997';|SELECT student_id FROM record WHERE yymm BETWEEN '199701' AND '199712';|
|SELECT * FROM employee WHERE salary * 12 < 10000;|SELECT * FROM employee WHERE salary < 10000 / 12;|

## Key Filter
Key Filter는 Key Range에는 포함되지 않지만 인덱스 키로 처리할 수 있는 조건이다.  
이러한 Key Filter가 WHERE 조건절에 포함되면 데이터 페이지에 접근하는 횟수를 줄일 수 있다.  
또한 Data Filter가 Key Filter로 적용될 수 있도록 인덱스에 칼럼을 추가하는 것도 방법이 될 수 있다.  

예를 들어 user 테이블에 (groupid, name)으로 구성된 인덱스 idx_1이 있는 상태에서 아래 질의를 수행한다고 하자.
```SQL
SELECT * FROM user  
WHERE groupid = 10  
AND age > 40;
```
groupid=10인 조건을 만족하는 레코드가 100건이고 그 중 age > 40인 레코드가 10건이라고 하면, 인덱스 스캔으로 100건의 OID를 가져온 후, 최악의 경우 데이터 페이지로 100회의 액세스를 수행할 것이다.   
그러나, idx_1 인덱스에 age 칼럼을 추가하여 (groupid, name, age)로 만들면 age > 40 조건이 Key Filter 조건으로 처리되어 인덱스 스캔으로 10건의 OID만 추출할 수 있다.

## Covering Index
인덱스가 하나의 질의를 모두 '커버'한 경우를 '커버링 인덱스(Covering Index)'라고 한다.  
만약 사용하는 인덱스로 SELECT 질의에 대한 결과를 모두 얻을 수 있는 상황이라면, 데이터 페이지에 저장되어 있는 레코드를 읽어오지 않아도 인덱스 키의 값만으로도 결과를 얻을 수 있다.  

질의에 커버링 인덱스가 활용되는지 확인하려면 다음 그림과 같이 질의 실행 계획에 covers라는 표시가 있는지 보면 된다.  

커버링 인덱스는 데이터 페이지를 읽지 않는다는 점, 그리고 해당 질의를 자주 사용하면 인덱스가 데이터베이스 버퍼에 캐시되어 있을 가능성이 높다는 점에서 디스크 I/O를 줄이는 데 큰 역할을 한다.   
따라서 레코드 크기에 비해 인덱스 키의 크기가 작고, 커버링 인덱스를 이용하는 질의가 자주 수행되는 것이 확실하다면, 커버링 인덱스를 사용하여 SELECT 질의 성능을 크게 향상시킬 수 있다.