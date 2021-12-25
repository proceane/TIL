# Time Complexity

## Definition
컴퓨터 과학에서 알고리즘의 시간 복잡도는 입력을 나타내는 문자열 길이의 함수로서 작동하는 알고리즘을 취해 시간을 정량화하는 것이다.  
알고리즘의 시간복잡도는 주로 [BigO표기법](https://github.com/proceane/TIL/blob/master/Develop/Algorithm/BigO.md)을 사용하여 나타낸다.([출처](https://ko.wikipedia.org/wiki/%EC%8B%9C%EA%B0%84_%EB%B3%B5%EC%9E%A1%EB%8F%84))  

## Kinds
상수 시간(Constant time)
- 어떤 알고리즘의 T(n) 값이 입력 크기에 구애받지 않는 값에 의해서 한정된다면, 이 알고리즘은 상수 시간(O(1) 시간이라고 쓰기도 함) 이라고 할 수 있음
- 예를 들면, 배열의 길이를 알고있을 때 배열의 특정 위치 값을 찾는 경우가 있음

로그 시간(Logarithmic time)
- T(n) = O(log n)
- 절반씩 줄어드는 로직이 반복될때 걸리는 시간
- 이진탐색, 이진트리

다항 로그 시간(Polylogarithmic time)
- T(n) = O((log n)<sup>k</sup>) 
- 예를 들면, 행렬 체인 곱은 병렬 랜덤 접근 기계 (Parallel Random Access Machine: PRAM)에서 다항 로그 시간에 해결할 수 있음

서브-선형 시간(Sub-linear time)
- T(n) = o(n), O(n1/2)
- 서브-선형 시간동안 작동하는 전형적인 알고리즘들은 병렬 프로세싱, 비표준 프로세싱을 사용하거나, 입력 구조에 대한 보장된 가정을 가짐
- 서브-선형 시간 알고리즘이라는 특정 용어는 알고리즘들이 전형적인 일렬 기계 모델에서 작동하며 입력에 대해 우선 가정을 허용하지 않음 

선형 시간(Linear time)
- O(n)
- 약식으로 충분히 큰 입력 크기에 대해서 수행시간이 입력 크기에 따라 선형적으로 증가함을 의미
- 예를 들면, 리스트의 모든 값을 더하는 로직

유사 선형 시간(Quasilinear time)
- T(n) = O(n logkn)
- 합병 정렬, 퀵 정렬, 힙 정렬, 고속 퓨리에 변환, Monge 배열 계산

선형 로그 시간(Linearithmic time)
- T(n) = O(n log n)
- 선형로그 항은 선형 항 보다는 빠르게 증가하지만, 1보다 큰 지수를 가진 n의 다항식보다는 느림

서브-이차 시간(Sub-quadratic time)
- T(n) = o(n<sup>2</sup>)
- 예를 들면, 간단히 비교 기반 정렬 알고리즘은 이차 (예를 들면, 삽입 정렬)이지만 더 심화된 알고리즘은 서브-이차 (예를 들면, 쉘 정렬)인 것을 찾을 수 있음

다항 시간(Polynomial time)
- T(n) = O(n<sup>k</sup>)
- 예를 들면 기본 사칙 연산 수행

초다항 시간(Superpolynomial time)
- ω(n<sup>c</sup>)
- 크기 n 의 입력값에 대해 2<sup>n</sup>단계 실행되는 알고리즘은 초다항 시간을 요구



