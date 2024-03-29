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


> Thread Safety는 '두 개 이상의 스레드가 서로 공유하는 리소스를 수정하는 로직을 포함하고 있을 때' 레이스 컨디션같이 데이터 정합성을 보장치
> 못하는 상황이 생길 수 있습니다. 이를 해결하기 위해 문제가 되는 *'공유 리소스'* 를
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

@ 결과
Synchronized만 설명했고, 추가적으로 Lock과 ReentrantLock등의 키워드 학습을 해볼 것을 조언 받았다.

```

##### 질문 1-3. 락을 사용했을 때의 문제점은 어떤게 있나요?
```
스타베이션, 데드락 등의 문제가 있을 수 잇습니다.

@TODO. 설명 작성

```


##### 질문 1-4. 근데요 .. 내가 지금 개발하는 클래스는 멀티 스레드를 목적으로 두고 개발하고 있진 않은데, 이 때에는 Thread safety를 고려 안해도 되는거겠죠?
```
고려해야합니다. 제 클래스를 나중에 누가 어떻게 쓸 지 모르기 때문입니다.
```

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


##### 질문 2-1. final 키워드에 대한 원칙에 대해서 어떻게 생각하세요?
```
모든 인자에 final을 붙이는 원칙에 대해 질문 주셨었는데, 이 부분은 더 알아봐야 할 거 같다.
```

...

<br>

#### 질문 3.Java 예외처리에 대해 설명하세요 (keyword: error, checked/unchecked exception, try/catch/finally, throws/throw)
```
프로그램이 운영되다 보면 예상치 못한 문제들이 발생할 수 있는데여. 그런 문제들을 잘 관리 하기 위해 지원되는 프로그래밍 기법을 예외처리라고 합니다.
그 중에서 예외를 탐지하고 처리하기 위한 try, catch, finally 같은 제어 문법이나 throws, thorw 같은 예외를 전파하는 문법들을 이용해서 컨트롤 할 수 있는 것들은
exception 클래스로 분류 하구요. 이 exception은 컴파일 단계에서 체크되는 checked exception과 실행 환경에서 확인되는 unchecked exception이 있습니다.


여기서 더 설명 하려면 예외 처리를 위한 try, catch, finally 같은 문법적인 부분이나 checked excpetion, unchecked exception 등에 대해 설명드릴 수 있을 거 같은데
어떤 부분에 대해서 더 설명 드리는게 더 좋으실지 ..

아니면 제가 설명한 것 중에 궁금하신 부분이 있으실까요?
```

<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/4291f044-4255-4edc-bb1e-b7dbaf494643" width=500>

##### 질문 3-1. try, catch, finally는 어떤 역할을 하나요?
```
try는 예외가 발생할 만한 부분을 정의해주는 역할을 하고 catch는 try로 묶인 영역에서 어떤 예외가 발생할건지에 대해 정의하고, 예외 발생시에 어떤 처리를 할건지를 정의하는 부분입니다.
finally는 예외발생 여부와 상관 없이 무조건 처리해야 하는 로직을 정의하는 역할입니다.
```

##### 질문 3-2. checkedException과 uncheckedException은 어떤 차이가 있나요?
```
checked Exception과 uncheckedException은 '컴파일 시점'에 예외처리를 강제하냐 아니냐의 차이가 있습니다. checked Exception 같은 경우는 컴파일 전에 JVM에 의해
'예외 처리'를 강제받게 됩니다. 예를들면 파일 입출력 로직을 처리할 때, IOException이 발생할 수 있기에 try-catch 구문을 통해 해당 예외를 핸들링 하던지 해당
메서드에 throws를 통해 메서드에서 해당 에러를 던질 것을 명시해야 합니다.

반면 unchecked exception은 처리가 강제되지 않고, 만약 실제 처리가 없더라도 발생시에 호출 스택을 따라 최상위 메서드까지 올라가서 jvm에 의해 처리됩니다.
```

##### 질문 3-3. 에러는 뭐에요 그럼?
```
java는 try-catch 같은 프로그래밍 기법을 적용해 핸들링하기엔 어려움이 있는, 치명적인 예외상황들을 Error 이라는 클래스로 분류해 다루고 있습니다.
굳이 이렇게 하는 이유는 치명적인 Error가 발생했을 때 좀 더 안전하게 쓰레드를 종료한다던지, 로깅을 통해 기록을 남기는데 활용하기 위해서 적용된 개념입니다.
```

...

<br>

#### 질문 4.Java Generic에 대해 설명 하시오 (keyword: 타입 파라미터, wildcard, type erasure, 장/단점, etc)
```
자바의 Generic은 클래스에서 사용되는 특정 타입을 내부적으로 미리 지정하는게 아니라 상황에 맞게 다형성 있게 활용하기 위한 개념이고
기존에 Object를 이용한 방법에서는 ClassCastingException 등의 문제들이 있었어서 일단 타입 파라미터랑 wildcard 라는 개념을 이용해서
실제 타입을 받아주고, 이런 작업을 컴파일 시에 대체해주는 방법을 통해 타입 안전성을 보장하는 개념입니다.
```


##### 질문 4-1. 타입 파라미터랑 wildcard에 대해 설명해주세요
```
타입 파라미터와 wildcard는 비슷한 역할을 수행하는데요, 차이가 있다면 wildcard는 메서드에 조금 더 특화된 개념입니다.
타입 파라미터는 class 우측에 <T> 같은 타입 값을 지정해 객체 생성시에 넘어온 타입 값으로 대체해 사용할 수 있게 하는 개념입니다.

wildcard 같은 경우는 메서드 정의 시에 인자나 리턴 타입을 정의할 때 '?'를 대신 넣어 어떤 타입이 들어오더라도 유연하게 사용할 수 있도록 도와주는 도구입니다.
```


##### 질문 4-2. type erasure는 무슨 의미에요?
```
저희가 타입 파라미터나 와일드 카드로 넘겨받은 실제 타입 정보는 컴파일 전까지는 가지고 있는데, 이제 컴파일 과정에서 제한된 정보에 맞춰 타입을 변경합니다.
제한된 정보가 없으면 Object,  --
```

<br>
