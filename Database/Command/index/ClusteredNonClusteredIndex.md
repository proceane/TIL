# Clustered, Non-Clustered Index

## Definition
Clustered 인덱스는 테이블 레코드가 데이터베이스에 저장되는 물리적인 순서를 나타낸다.([출처](https://www.spotlightcloud.io/blog/when-to-use-clustered-or-non-clustered-indexes-in-sql-server))    
개발자가 설정하는 것이 아닌, DBMS가 설정하는 인덱스이다.([출처](https://www.kyungyeon.dev/posts/66))
기본적으로 클러스터 인덱스는 기본키 열에 생성된다.  

사용자 지정 클러스터 인덱스도 만들수 있지만 기존에 기본키에 의해 생성된 클러스터 인덱스를 제거해야한다고 한다.  

Non-Clustered 인덱스는 DBMS가 생성하지 않고 개발자나 DBA가 생성하는 모든 인덱스를 의미한다.  
보통 검색작업의 속도를 높이는데 사용된다.  
클러스터형 인덱스와는 달리 물리적인 순서는 정의하지 않고, 한 테이블 당 여러개가 있을 수 있다.  

## Using
인덱스를 여러개 사용하는경우엔 비클러스터형 인덱스  
디스크 공간이 여유롭지 않다면 클러스터형 인덱스  
삽입 및 수정이 자주 일어나는 컬럼에는 디스크 공간이 여유롭다면 비클러스터 인덱스

