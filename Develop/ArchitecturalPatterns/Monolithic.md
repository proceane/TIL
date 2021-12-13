# Monolithic Architecture

## Definition
모놀리식 아키텍쳐(MA)란, 마이크로서비스 아키텍쳐(MSA)의 반대되는 개념으로 전통의 아키텍쳐를 지칭하는 의미로 생겨난 단어이다.  
하나의 서비스 또는 애플리케이션이 하나의 거대한 아키텍쳐를 가질 때 모놀리식하다고 한다.([출처](https://m.blog.naver.com/dktmrorl/221863498991))

## Pros and Cons
장점
- 어느 기능이든 개발 환경이 같아서 복잡하지 않다.
- 쉽게 고가용성 서버를 만들 수 있다.
- End-to-End 테스트가 용이하다.

단점
- 프로젝트 크기가 너무 커서 어플리케이션 구동 시간이 늘어나고 빌드, 배포 시간이 길어진다.
- 작은 수정사항이 있어도 전체를 수정해야 한다.
- 많은 양의 코드로 인해 이해가 어렵다.
- 일부의 오류가 전체에 영향을 미친다.
[출처](https://lion-king.tistory.com/entry/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C-%EC%84%9C%EB%B9%84%EC%8A%A4-vs-%EB%AA%A8%EB%86%80%EB%A6%AC%EC%8B%9D-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-MicroService-vs-Monolithic-Architecture-%EA%B0%84%EB%8B%A8-%EC%86%8C%EA%B0%9C-%EB%B0%8F-%EC%A3%BC%EA%B4%80%EC%A0%81-%EC%9D%98%EA%B2%AC)

### 여담
CMS는 모놀리식인가?  
큰 프로젝트가 있다면 어떻게 나누는 것이 효율적일까?

