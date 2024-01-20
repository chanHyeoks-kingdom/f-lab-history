# 3주차 회고

[3주차 멘토링 질문에 대한 내 답변.pdf](-)
```
이번주는 4가지 질문에 대한 답을 찾는 과정이었다.
1. Thread Safety와 동기화(synchronization)에 대해 설명 하시오 (키워드: lock, synchronized, deadlock, ThreadLocal, etc)
2. Static, Final 키워드에 대해 설명 하시오 (키워드: 사용이유, 장단점, etc)
3. Java 예외처리에 대해 설명하세요 (keyword: error, checked/unchecked exception, try/catch/finally, throws/throw)
4. Java Generic에 대해 설명 하시오 (keyword: 타입 파라미터, wildcard, type erasure, 장/단점, etc)

멘토링을 통해 얻은 인사이트를 아래와 같은 목차로 정리해보려한다.
(내 생각을 통해 2차적으로 가공된 결론이어서 멘토링 내용과 연관성이 없을 수 있습니다.)
```

> [@고민해볼 관점]
> 
> (1) Deadlock이 발생하는 상황들과 방지법
> (2) 동기화 문제 종류 및 발견 방법과 방지하는 법, 실제 코딩 작성시 고려 사항
> (3) Static 키워드 사용시 주의점, 메모리 누수
> (4) Final 키워드 선택 원칙
> (5) Catch block, custom exception과 throws/throw 사용에 대한 원칙
----------------------------------------------

<br>

## 1. JVM 메모리 구조에 대한 설명


```
JVM은 프로세스지만 효율적인 메모리 관리를 위해 내부적으로 가상 메모리를 관리한다. 내부적으로 [method, heap, stack, pc, native method stack]
으로 아래 그림처럼 다섯가지 영역으로 나뉜다. 
```

<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/16c12151-84fc-496b-87ce-b6c491b26866" width="500">

* [그림1]. JVM 가상 메모리 구조의 PC 영역

<br>

--- 
