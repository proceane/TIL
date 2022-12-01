# Java Platform

만들고자 하는 프로젝트가 있는데, 참고하고 있는 프로젝트에서 jarkarta로 시작하는 패키지를 사용하는 것을 확인하였다.  
찾아보니 java 플랫폼이라고 나와있어서 이에 대해 정리해 보려고 한다.  

## Java EE
Java는 여러 플랫폼을 가지고 있다.  
Java SE, Java EE, Java ME, JavaFX 총 4가지 플랫폼을 가지고 있다.  

Java SE는 Standard Edition으로, 가장 대중적인 자바 플랫폼이다.  
`java.lang`, `java.util` 등 흔히 자바 언어라고 하는 대부분의 패키지가 포함되어있는 에디션이다.  

Java EE는 Enterprise Edition으로, Java SE 위에 구축된 플랫폼이다.  
확장 가능하고 안정적이며 안전한 대규모 다중 계층 네트워크 애플리케이션을 개발하고 실행하기 위한 API 런타임 환경을 제공한다.  

Java ME는 모바일용 플랫폼, JavaFX는 경량화 플랫폼이다.

## J2EE
J2EE는 Java 2 Enterprise Edition이다. 
자바 기술로 기업환경의 어플리케이션을 만드는 데 필요한 스펙들을 모아둔 스펙 집합이다.

J2EE를 만든 것은 Sun Microsystems이지만 다른 벤더들도 J2EE 스펙을 구현할 수 있고, J2EE를 개선하는 과정에도 활발하게 참여하기때문에 Sun의 독점적인 기술이라기보다는 Java 진영의 개발자들이 다같이 만들어가고 공유하는 기술로 보는 것이 정확할 것이다.  

구성요소는 Servlet, JSP, EJB, RMI, JNDI, JDBC, JCA, JMS 등이 있다.  

## Jarkarta EE
자바를 이용한 서버측 개발을 위한 플랫폼이다.
PC에서 동작하는 표준 플랫폼인 Java SE에 부가하여 웹 어플리케이션 서버에서 동작하는 장애복구 및 분산 멀티티어를 제공하는 자바 소프트웨어의 기능을 추가한 서버를 위한 플랫폼이다.  

이전에는 J2EE로 불렸고, 버전 5.0 이후로 Java EE로 개칭되었으며 2017년 프로젝트가 이클립스 재단으로 이관됨에 따라 Jarkarta EE로 변경되었다.

Jarkarta EE 스펙에 따라 제품으로 구현한 것을 웹 어플리케이션 서버(WAS)라고 불린다.

### Reference

[https://www.baeldung.com/java-enterprise-evolution](https://www.baeldung.com/java-enterprise-evolution)  
[https://doozi316.github.io/java/2020/07/01/WEB20/](https://doozi316.github.io/java/2020/07/01/WEB20/)  
[https://devbox.tistory.com/entry/실행](https://devbox.tistory.com/entry/%EC%8B%A4%ED%96%89)  
[https://ko.wikipedia.org/wiki/자바_플랫폼,_스탠더드_에디션](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%ED%94%8C%EB%9E%AB%ED%8F%BC,_%EC%8A%A4%ED%83%A0%EB%8D%94%EB%93%9C_%EC%97%90%EB%94%94%EC%85%98)  
[https://docs.oracle.com/javaee/6/firstcup/doc/gkhoy.html](https://docs.oracle.com/javaee/6/firstcup/doc/gkhoy.html)  
[https://gyrfalcon.tistory.com/entry/J2EE](https://gyrfalcon.tistory.com/entry/J2EE)  
[https://ko.wikipedia.org/wiki/자카르타_EE](https://ko.wikipedia.org/wiki/%EC%9E%90%EC%B9%B4%EB%A5%B4%ED%83%80_EE)  
