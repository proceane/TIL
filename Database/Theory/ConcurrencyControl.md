# Concurrency Control

## Definition
동시성 제어는 가능한 빠른 조회와 동시에 병행되는 동작의 정확한 결과가 발생하는 것을 보증한다.([출처](https://ko.wikipedia.org/wiki/%EB%8F%99%EC%8B%9C%EC%84%B1_%EC%A0%9C%EC%96%B4))
또 다른 정의로 다중 사용자 환경을 지원하는 데이터베이스 시스템에서 동시에 실행되는 여러 트랜잭션 간의 간섭으로 문제가 발생하지 않도록 트랜잭션의 실행 순서를 제어하는 기법이다.([출처](https://medium.com/pocs/%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%A0%9C%EC%96%B4-%EA%B8%B0%EB%B2%95-%EC%9E%A0%EA%B8%88-locking-%EA%B8%B0%EB%B2%95-319bd0e6a68a))

데이터베이스에서 사용하는 동시성 제어 기법인 잠금 기법에 대해 정리해보고자 한다.

## Locking
