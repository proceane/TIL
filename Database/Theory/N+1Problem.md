# N + 1 Problem

## Definition
N + 1문제란 쿼리 1번으로 N건을 가져왔는데, 관련 컬럼을 얻기 위해 쿼리를 N번 더 수행하는 문제이다.
([출처](https://zetawiki.com/wiki/N%2B1_%EC%BF%BC%EB%A6%AC_%EB%AC%B8%EC%A0%9C))

## Example
예를 들어, 사람 100명의 주소를 구해야 한다고 가정한다.

Person테이블과 Address테이블에는 각각 100건의 데이터가 있다.  
먼저 Person 테이블에서 전체 데이터를 찾는다. 
```sql
SELECT * FROM PERSON p;
``` 
실행 결과, 100명의 사람들의 데이터가 조회될 것이다.

그리고 Person테이블의 키 값으로 Address 테이블에서 각 사람의 주소를 찾는다.
```sql
SELECT * FROM ADDRESS a WHERE a.person_idx = ?;
```
해당 쿼리를 사람 수에 맞게 100번 수행해야 사람들 각각의 주소를 구할 수 있다.

이렇게 되는 경우, 쿼리를 많이 수행하게 되어 성능 상에 문제가 생길 가능성이 높아진다.

## Solve
위의 경우는 Join을 사용하면 해결된다.
```sql
SELECT 
  p.name, a.address 
FROM PERSON p
LEFT JOIN ADDRESS a ON p.person_idx = a.person_idx; 
```

JPA에서 해당 문제를 막기 위해서는 여러 방법이 존재한다.([해결 방법 참고 위치](https://velog.io/@geunwoobaek/JPA-N1%EB%AC%B8%EC%A0%9CLazy-Loading%EB%AC%B8%EC%A0%9C))
해당 출처에서는 Fetch Join과 @EntityGraph를 소개하고 있다.

### 여담
쿼리를 직접 작성한다면 그냥 Join을 사용하면 되니까 N + 1문제에 영향을 받을 일이 크게 없을 것 같다.  
하지만 JPA는 내가 쿼리를 작성하지 않아서 N + 1이 발생할 가능성이 높다.  
그래서 내가 원하는 쿼리를 만들어내기 위해 어노테이션같은 것으로 따로 설정해야할 사항이 있는 것 같다.