# Comparator<T>

## Definition

java.util에 포함된 클래스이며, 
[Document](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)에 명시된 정의는 다음과 같다.
> A comparison function, which imposes a total ordering on some collection of objects

공식문서에 따르면 Comparator는 일부 객체 컬렉션 전체 순서를 부과하는 비교 함수라고 알려주고 있다.  
또한, Comparator는 Class구현 외에 Collections.sort()나 Arrays.sort()와 같은 정렬 메소드의 인자로 사용할 수 있다.


## Method
`int compare(T o1, T o2)`  
해당 메소드만 구현하면 된다.  
Comparable과 다른 점은 자기 자신이 아닌, 두 객체를 비교한다는 차이가 있다.

Comparator를 구현한 클래스 변수를 통해 사용할 수 있으며
```java
Student a = Student.builder().height(150).build();
Student b = Student.builder().height(155).build();
a.compare(a, b) //두 학생의 키를 비교
```

정렬 메소드의 인자로도 사용할 수 있다.
```java
List<Student> students = new ArrayList<Student>();
students.add(a);
students.add(b);

// 키 순서대로 정렬
Collections.sort(students, new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return s1.height - s2.height;
    }
});
```

Comparable과 마찬가지로 양수를 반환하면 두 객체의 위치가 바뀐다.

## Pros and Cons 

직접 사용했을때 느꼈던 장단점을 적어보려 한다.  
* 정렬 기준을 자유롭게 정할 수 있다.  
또, 같은 리스트를 서로 다른 기준으로 정렬할 수도 있다.  
그래서 Comparable로 구현한 기준과 다르게 정렬하고싶을 때, 해당 클래스를 사용하면 기준을 바꿔서 정렬할 수 있다.  

단점은  
* 코드가 길어진다.  
클래스 구현이 아닌 메소드 내에서 사용한다면, override된 메소드로 인해 코드의 길이가 길어진다.

## Conclusion
Comparator는 Comparable과는 다르게 클래스 구현 뿐만 아니라 정렬메소드의 인자로서도 사용할 수 있다. 그리고 자기 자신과 다른 객체를 비교하는 것이 아닌, 서로 다른 두 객체를 비교한다.

### 여담
만약 Comparator를 많이 사용해야하는 상황이라면 정렬기준을 하나씩 만들어놓고 가져다가 쓸수있지 않을까?   
비슷한 내용을 디자인패턴에서 본 것 같다.