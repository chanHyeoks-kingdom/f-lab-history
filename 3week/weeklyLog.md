# WEEK 3

[Guide Thread 1.](https://www.baeldung.com/java-thread-safety)
[Guide Thread 2.](https://velog.io/@junttang/SP-5.4-Thread-Safe-Functions)
[Tip 1.](https://developer-ellen.tistory.com/205)
[Tip 2.](https://velog.io/@guswlsapdlf/Java-Thread-Safety-Unsafety)
['final static' vs 'static final'](https://www.baeldung.com/java-static-final-order)

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
static 키워드는 프로그램의 시작부터 끝까지 메모리에서 지워지지 않는 '정적'인 무언가를 만들고 싶을 때 사용하는 키워드입니다.
이렇게 정적으로 생성된 것들은 어디서든 편리하게 접근할 수 있고 동일한 데이터들의 중복 생성을 막을 수 있다는 장점이 있습니다. 
사실 이렇게 '어디서든 접근 가능'하다는 특징은 코드간의 side effect를 일으킬 수 있는데 이 때 final 같은 키워드들을 이용해
'변경할 수 없는' 상태로 만들어 사이드 이펙트를 줄일 수 있습니다.

방금 설명드린 final 키워드는 대상을 '수정 불가능한' 상태로 만드는데 목적이 있습니다. 변수의 경우 값 재할당 불가, 메소드의 경우
오버라이드 불가, 클래스의 경우 상속 불가의 특징을 가지게됩니다.
```


##### 질문 1-1. 꼬리질문
```
..
```

...

<br>

#### 질문 3.Java 예외처리에 대해 설명하세요 (keyword: error, checked/unchecked exception, try/catch/finally, throws/throw)
```
자바는 예상치 못한 문제가 발생했을 때 '예외'개념을 이용해 해당 문제를 래핑해줍니다. 문제가 발생한 시점부터 상위 메서드로 계속해서 해당 예외를 전파하는데
최상위 메서드에 도달했을 때 까지도 예외에 대한 처리가 없으면 JVM은 스택 트레이스의 내용을 출력합니다. 바로 이게 우리가 문제가 생겼을 때 흔히 보는 스택
트레이스가 이러한 과정을 통해 출력되는 것입니다.

이렇게 JVM 수준에서 진행되는 예외 처리 말고도 사용자가 직접 예외를 처리할 수 있는데 처음 던져진 에러들을 중간에서 catch로 잡아내는 것이 그 방법입니다.
에러가 날 만한 부분을 try 블록으로 묶은 뒤 catch, 그러니까 에러가 실제로 발생했을 때 처리하는 구문을 작성해주는 방식으로 에러를 핸들링할 수 있게 됩니다.
```

<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/4291f044-4255-4edc-bb1e-b7dbaf494643" width=500>




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
