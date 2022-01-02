# Atomic Variables and ConcurrentMap
[해당 문서](https://winterbe.com/posts/2015/05/22/java8-concurrency-tutorial-atomic-concurrent-map-examples/)에 있는 내용을 정리  

## AtomicInteger
`java.concurrent.atomic`패키지에는 원자적 작업을 수행하는 데 유용한 여러 클래스가 포함되어 있다.  
이전에 `synchronized`에서 보았던 것처럼 키워드나 잠금을 사용하지 않고 여러 스레드에서 병렬로 안전하게 작업을 수행할 수 있을 때 작업은 원자적이라고 한다.  

내부적으로 원자 클래스는 대부분의 최신 CPU에서 직접 지원하는 원자 명령인 CAS(비교 및 교환)를 많이 사용한다.  
이러한 명령은 일반적으로 잠금을 통한 동기화보다 훨씬 빠르다.  
따라서 변경 가능한 단일 변수를 동시에 변경해야 하는 경우를 대비하여 잠금보다 원자 클래스를 선호하는 것이다.  

`Integer` 대체를 위해 `AtomicInteger`를 사용하여 변수에 대한 액세스를 동기화하지않고 스레드 안전 영지에서 동시에 수를 증가할 수 있다.  

## LongAdder
`AtomicLong`에 대한 대안으로 `LongAdder`를 사용하여 숫자에 값을 연속적으로 추가할 수 있다.  

이 클래스는 일반적으로 여러 스레드의 업데이트가 읽기보다 더 일반적일때 원자 번호보다 선호된다.  
이는 통계 데이터를 캡쳐할 때 종종 발생한다.  
예를 들어 웹 서버에서 제공되는 요청 수를 계산하려는 경우이다.  
LongAdder의 단점은 변수 집합이 메모리에 유지되기 때문에 메모리 소비가 더 많다는 것이다.  

## LongAccumulator
`LongAccumulator`는 `LongAdder`보다 일반화된 버전이다.  
`LongAccumulator` 클래스는 간단한 추가 작업을 수행하는 대신 다음 코드 샘플에 람다식을 기반으로 `LongBinaryOperator`를 빌드한다.   

## ConcurrentMap
인터페이스 `ConcurrentMap`은 맵 인터페이스를 확장하고 가장 유용한 동시 컬렉션 유형 중 하나를 정의한다.  
Java 8은 이 인터페이스에 새로운 메소드를 추가하여 함수형 프로그래밍을 도입한다.  

## ConcurrentHashMap
`ConcurrentHashMap`은 Map에서 병렬 작업을 수행하는 몇가지 새로운 방법으로 더욱 향상되었다.  

병렬 스트림과 마찬가지로 이러한 메소드는 Java 8의 ForkJoinPool.commonPool()을 통해 사용할 수 있는 특수 ForkJoinPool을 사용한다.  

이 풀은 사용 가능한 코어 수에 따라 미리 설정된 병렬 처리를 사용한다.  
내 컴퓨터에서 4개의 CPU코어를 사용할 수 있으므로 3개의 병렬 처리가 발생한다.  

Java 8에는 forEach, 검색 및 축소의 세가지 병렬 작업이 도입되었다.  
이러한 각 작업은 키, 값, 항목 및 키-값 쌍 인수가 있는 함수를 허용하는 네가지 형식으로 사용할 수 있다.  

이러한 모든 방법은 parallelismThreshold라는 공통 첫번째 인수를 사용한다.  
이 임계값은 작업을 병렬로 실행해야 하는 경우 최소 컬렉션 크기를 나타낸다.  

예를 들어 임계값 500을 전달하고 맵의 실제 크기가 499인 경우 단일 스레드에서 작업이 순차적으로 수행된다.  