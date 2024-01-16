# WEEK 3

```
[이 주의 과제]
1. Thread Safety와 동기화(synchronization)에 대해 설명 하시오 (키워드: lock, synchronized, deadlock, ThreadLocal, etc)
2. Static, Final 키워드에 대해 설명 하시오 (키워드: 사용이유, 장단점, etc)
3. Java 예외처리에 대해 설명하세요 (keyword: error, checked/unchecked exception, try/catch/finally, throws/throw)
4. Java Generic에 대해 설명 하시오 (keyword: 타입 파라미터, wildcard, type erasure, 장/단점, etc)
5. Java Collection에 대해 설명 하시오 (keyword: Set, Map, List, Queue, Stack , etc)
6. Java Synchronized Collection과 Concurrent Collection을 비교하시오.
```

> [@고민해볼 관점]
> 
> Thread Safety를 항상 고려해서 코딩을 해야하는 이유, Deadlock이 발생하는 상황들과 방지법, 동기화 문제 종류 및 발견 방법과 방지하는 법, 실제 코딩 작성시 고려 사항, Static 키워드 사용시 주의점(메모리 누수), Final 키워드 선택 원칙

-----


## [경계성 질문]

## 질문 1.Thread Safety와 동기화(synchronization)에 대해 설명 하시오 (키워드: lock, synchronized, deadlock, ThreadLocal, etc)
```
JVM은 메모리의 효율성과 안정성등의 이점을 위해 내부적으로 가상의 메모리 공간을 분리하는데요 이런 영역은 '공유되는 영역'과 '공유되지 않는 영역'으로 나뉘는데 혹시 어떤 것 부터 설명드리면 좋을까요?
```


##### 질문 1-1. 꼬리질문
```
..
```

...

++ 여기 metaspace 영역에 대해 설명 추가
<br>


