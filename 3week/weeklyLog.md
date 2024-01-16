# WEEK 3

[Tip 1.](https://developer-ellen.tistory.com/205)
[Tip 2.](https://velog.io/@guswlsapdlf/Java-Thread-Safety-Unsafety)


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


## [자문자답]

#### 질문 1.Thread Safety와 동기화(synchronization)에 대해 설명 하시오 (키워드: lock, synchronized, deadlock, ThreadLocal, etc)
```
A. Thread Safety는 두 개 이상의 작업에 서로 공유하는 자원을 활용할 때 데이터 정합성을 보장토록 하는 걸 의미합니다.
```


> Thread Safety는 '두 개 이상의 스레드가 서로 공유하는 리소스를 수정하는 로직을 포함하고 있을 때' 레이스 컨디션이나 
> 스타베이션 같이 데이터 정합성을 보장치 못하는 상황이 생길 수 있습니다. 이를 해결하기 위해 문제가 되는 *'공유 리소스'* 를
>
> (1)보호 하거나 (2)아예 없애도록
> 
> 해서 데이터 정합성을 보장하는게 *Thread Safety*입니다.

##### 질문 1-1. 보호하는 방법에는 뭐가 있나요?
```
'Lock'을 활용해 작업 흐름을 안전하게 관리하거나 '불변객체'들을 이용해 '애초에 수정치 못하도록' 하는 방법이 있습니다.
```

##### 질문 1-2. 락을 활용해 작업 흐름을 안전하게 관리하는 방법은 뭐가 있나요?
```
자바에선 Synchronized Blocks/Methods, Atomic Operations, 세마포어, 조건 변수 등을 구현해 보장할 수 있습니다.

@TODO. 설명 작성

```


##### 질문 1-1. 근데요 .. 내가 지금 개발하는 클래스는 멀티 스레드를 목적으로 두고 개발하고 있진 않은데, 이 때에는 Thread safety를 고려 안해도 되는거겠죠?
```
..
```

...

<br>

#### 질문 2.Static, Final 키워드에 대해 설명 하시오 (키워드: 사용이유, 장단점, etc)
```
JVM은 메모리의 효율성과 안정성등의 이점을 위해 내부적으로 가상의 메모리 공간을 분리하는데요 이런 영역은 '공유되는 영역'과 '공유되지 않는 영역'으로 나뉘는데 혹시 어떤 것 부터 설명드리면 좋을까요?
```


##### 질문 1-1. 꼬리질문
```
..
```

...

<br>

#### 질문 3.Java 예외처리에 대해 설명하세요 (keyword: error, checked/unchecked exception, try/catch/finally, throws/throw)
```
JVM은 메모리의 효율성과 안정성등의 이점을 위해 내부적으로 가상의 메모리 공간을 분리하는데요 이런 영역은 '공유되는 영역'과 '공유되지 않는 영역'으로 나뉘는데 혹시 어떤 것 부터 설명드리면 좋을까요?
```


##### 질문 1-1. 꼬리질문
```
..
```

...

<br>

#### 질문 4.Java Generic에 대해 설명 하시오 (keyword: 타입 파라미터, wildcard, type erasure, 장/단점, etc)
```
JVM은 메모리의 효율성과 안정성등의 이점을 위해 내부적으로 가상의 메모리 공간을 분리하는데요 이런 영역은 '공유되는 영역'과 '공유되지 않는 영역'으로 나뉘는데 혹시 어떤 것 부터 설명드리면 좋을까요?
```


##### 질문 1-1. 꼬리질문
```
..
```

...

<br>

#### 질문 5. Java Collection에 대해 설명 하시오 (keyword: Set, Map, List, Queue, Stack , etc)
```
JVM은 메모리의 효율성과 안정성등의 이점을 위해 내부적으로 가상의 메모리 공간을 분리하는데요 이런 영역은 '공유되는 영역'과 '공유되지 않는 영역'으로 나뉘는데 혹시 어떤 것 부터 설명드리면 좋을까요?
```


##### 질문 1-1. 꼬리질문
```
..
```

...

<br>

#### 질문 6. Java Synchronized Collection과 Concurrent Collection을 비교하시오.
```
JVM은 메모리의 효율성과 안정성등의 이점을 위해 내부적으로 가상의 메모리 공간을 분리하는데요 이런 영역은 '공유되는 영역'과 '공유되지 않는 영역'으로 나뉘는데 혹시 어떤 것 부터 설명드리면 좋을까요?
```


##### 질문 1-1. 꼬리질문
```
..
```

...

<br>
