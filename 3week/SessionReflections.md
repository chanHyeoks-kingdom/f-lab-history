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

## 1. 자바에서 Thread Safety와 동기화를 어떻게 준수할까요?


```
먼저 `thread safety`라는 건 여러 스레드를 이용해 병렬 작업을 수행할 때 작업의 결과로 데이터 정합성이 헤쳐지지 않는 상태를 유지하는 것을 말합니다. 여러 스레드를 동시에 사용할 때
우리는 '레이스 컨디션'같은 상황등을 통해 데이터 정합성을 보장하지 못할 수 있는데 대표적으로 lock을 이용해 공유 자원에 대한 동시접근을 컨트롤해 이 문제를 해결할 수 있습니다.

특히 자바에서는 이런 동기화를 위해 synchronized라는 키워드를 제공합니다.
```
[[관련 내용]](https://blog.naver.com/cksgurwkd12/223336505526)

<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/16c12151-84fc-496b-87ce-b6c491b26866" width="500">

* [그림1]. JVM 가상 메모리 구조의 PC 영역

<br>

--- 
