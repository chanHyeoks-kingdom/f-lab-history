# WEEK 2

```
[이 주의 과제]
1. JVM 메모리 구조에 대해 설명 하시오 (키워드: Method Area, Heap, Stack, etc)
2. 자바 Garbage Collector 동작원리와 과정에 대해 설명 하시오 (키워드: Heap Memory, Minor/Major GC, GC algorithm, etc)
3. Thread Safety와 동기화(synchronization)에 대해 설명 하시오 (키워드: lock, synchronized, deadlock, ThreadLocal, etc)
4. Static, Final 키워드에 대해 설명 하시오 (키워드: 사용이유, 장단점, etc)
```

-----


## [경계성 질문]

## 질문 1.JVM 메모리 구조에 대해 설명 하시오 (키워드: Method Area, Heap, Stack, etc)
```
JVM은 메모리를 용도에 맞게 분리해 다양한 성능상의 이점과 보안상의 이점을 가져가는데 대표적인 가상 메모리 영역의 예시는 "Method", "Heap", "Stack" 영역이 있습니다.

Method 영역은 정적 정보들을 저장하는 곳
Stack은 쓰레드별로 하나씩 생성되서 해당 쓰레드의 흐름을 후입선출 방식으로 따라가는 구조입니다.
Heap 영역은 동적 정보들을 저장하는 곳, 그러니까 객체 생성시에 이와 관련된 값들이 저장되는 곳이며 GC의 영향을 받습니다.  -->  애를 마지막에 설명하고 GC 동작 원리 쪽으로 넘어가는 방향

```

##### 질문 1-1.각 영역이 하는 일이 뭔데요?
```

```

<br>



## 질문 2. 자바 Garbage Collector 동작원리와 과정에 대해 설명 하시오 (키워드: Heap Memory, Minor/Major GC, GC algorithm, etc)
```
가비지 컬렉터는 정확이 뭘까?  가비지 컬렉터는 JVM 이라는 프로그램에서 나오는 하나의 쓰레드? 응 .. 그래서 별도 쓰레드가 있는 하나의 작업 흐름이고, JVM 실행 이후부터
계속 Heap 영역을 주시하면서 회수해야 할 객체들을 보면서 Minor GC에서 Major GC로 옮기는 등 .. 오래된 객체들을 관리하기도 해

------
JVM(프로세스)
- 메인 쓰레드
- JVM용 쓰레드
- * 가비지 컬렉터용 쓰레드 *
이렇게 될거야
------


```

##### 질문 2-1.각 영역이 하는 일이 뭔데요?
```

```

<br>


## 질문 3.Thread Safety와 동기화(synchronization)에 대해 설명 하시오 (키워드: lock, synchronized, deadlock, ThreadLocal, etc)
```
그러게 .. 쓰레드는 동기화 잘 해줘야 한다고 하는 거 같은디, 그 중에 방법이 저 synchronized였던게 기억이 나네 그려 ..

```

##### 질문 3-1.각 영역이 하는 일이 뭔데요?
```

```

<br>


## 질문 4.Static, Final 키워드에 대해 설명 하시오 (키워드: 사용이유, 장단점, etc)
```
Static 쓰면 method 영역에 들어갈거야? 근데 클래스도 마찬가지 아녀? 클래스 필드도 결국 그냥 method 갔더 거 같은디 .

```

##### 질문 4-1.각 영역이 하는 일이 뭔데요?
```

```

<br>


