# Concurrency
[해당 문서](https://winterbe.com/posts/2015/04/07/java8-concurrency-tutorial-thread-executor-examples/)에 있는 내용을 정리

## Thread and Runnable
모든 최신 운영체제는 프로세스와 스레드를 통해 동시성을 지원한다.  
자바 프로그램을 실행하면 운영체제가 다른 프로그램과 병렬로 실행되는 새 프로세스를 생성한다.  
이러한 프로세스 내에서 스레드를 활용하여 코드를 동시에 실행할 수 있으므로 CPU의 코어를 최대한 활용할 수 있다.  

Java는 1.0부터 스레드를 지원한다.  
새 스레드를 시작하기 전에 이 스레드에서 실행할 코드를 지정해야하는데 이를 Task라고 한다.  

순서는 비결정적이므로 동시 프로그래밍을 더 큰 응용 프로그램에서 복잡한 직업으로 만든다.

스레드는 특정 기간동안 절전 모드로 전환될 수 있다.  

Thread 클래스 작업은 매우 지루하고 오류가 발생하기 쉽다.  
이러한 이유로 2004년에 java 5와 함께 Concurrency API가 도입되었다.  
`java.util.concurrent`패키지에 동시 프로그래밍을 처리하는 데 유용한 많은 클래스들을 포함한다.  
그 이후로 Concurrency API는 모든 새로운 Java릴리즈와 함께 향상되었으며 Java 8조차도 동시성을 처리하기 위한 새로운 클래스와 메소드를 제공한다.

## Executors


## Scheduled Executors