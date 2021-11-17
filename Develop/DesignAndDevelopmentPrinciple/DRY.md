# DRY

## Definition
DRY란 Don't repeat yourself의 약자로, 반복하지 마라는 의미다.([위키백과](https://ko.wikipedia.org/wiki/%EC%A4%91%EB%B3%B5%EB%B0%B0%EC%A0%9C))

처음 DRY를 접하고 떠오른 개념은 [바퀴의 재발명](https://ko.wikipedia.org/wiki/%EB%B0%94%ED%80%B4%EC%9D%98_%EC%9E%AC%EB%B0%9C%EB%AA%85)이다.  
다만 바퀴를 다시 발명하지 말라는 원칙은 이미 잘 만들어져 있는 것을 또 만들지 말라는 뜻이고,
DRY는 코드의 중복을 줄이라는 뜻에 더 가까운 듯 하다.

만약 2가지 작업을 하는 코드가 있을때, 2가지 중 1가지만 필요한 로직이 있을때 그 1가지를 위해 코딩을 다시 해야한다.  
그런 경우에는 DRY 원칙이 위배된다.