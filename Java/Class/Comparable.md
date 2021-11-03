# Comparable<T>

## Definition

[Document](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)에 명시된 Comparable의 정의는 다음과 같다.  

>This interface imposes a total ordering on the objects of each class that implements it.

해석하자면, Comparable을 구현한 각 클래스의 객체에 전체 순서를 부여한다는 의미이다.  
해당 인터페이스를 구현한 클래스를 정렬하려 할 때, Comparator 없이 정렬이 가능하다고 한다.

## Method
`int compareTo(T o)`
>Compares this object with the specified object for order.  

이 객체와 지정된 객체를 비교한다.  
다시 말하자면, 나 자신과 다른 객체를 비교할 수 있게 해주는 메소드이다.

## Pros and Cons
사람마다 다를수는 있지만, 직접 Comparable을 사용했을 때 느꼈던 장단점을 적어보려 한다.
   
* 코드가 간결해진다.  
클래스에 Comparable을 구현하면 `Collections.sort(list)`만 사용하여도 Comparator 없이 정렬이 가능하다.

* 정렬 기준을 다양하게 정할 수 있다.  
객체에 있는 값이라면 무엇이든 정렬의 기준이 될 수 있다.  


장점도 있지만, 단점도 있다.  
* 다른 정렬기준을 사용하기 힘들다.  
compareTo 메소드는 하나 밖에 없어서 여러가지 정렬기준을 만들고 싶을때는 적합하지 않다.

## Conclusion
Comparable은 클래스에 구현하여 사용하는 인터페이스이며, 해당 클래스가 구현된 객체를 정렬하려 할 때 따로 정렬 기준을 생성하지 않아도 Comparable에 있는 comparaTo메소드에 의해 객체가 정렬된다.


### 여담
첫 TIL을 Java의 Comparable 클래스로 하게 되었다.  
처음부터 거창한 내용을 작성하기보다는 자주 사용하지만 애매했던 것을 적어보고 싶었다.  
그래서 자주 햇갈렸던 Comparable로 정하였다.