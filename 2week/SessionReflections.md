# 2주차 회고

[2주차 멘토링 질문에 대한 내 답변.pdf](-)
```
이번주는 4가지 질문에 대한 답을 찾는 과정이었다.
(1) JVM 메모리 구조에 대한 설명 (2) 자바 Garbage Collector에 대해서 (3) Thread Safety와 동기화에 대해서 (4) Static, Final에 대해서

멘토링을 통해 얻은 인사이트를 아래와 같은 목차로 정리해보려한다.
(내 생각을 통해 2차적으로 가공된 결론이어서 멘토링 내용과 연관성이 없을 수 있습니다.)
```

```
# IDX
(1) JVM 메모리 구조에 대한 설명
(2) 자바 Garbage Collector에 대해서
(3) Thread Safety와 동기화에 대해서
(4) Static, Final에 대해서
```
<br>



> [@고민해볼 관점]
> 
> GC 각 알고리즘의 차이점, Garbage Collection 모니터링과 메모리 설정시 고려 사항, GC 모니터링 툴(jstat, visualvm, HPJMeter, YourKit),
> GC 튜닝(설정 조정이 minot/major GC에 미치는 영향), minor GC vs major GC 과정과 차이점, 메모리 누수 발견 및 해결 방법,
>  Thread Safety를 항상 고려해서 코딩을 해야하는 이유, Deadlock이 발생하는 상황들과 방지법, 동기화 문제 종류 및 발견 방법과 방지하는 법,
> 실제 코딩 작성시 고려 사항, Java Stacktrace란, Static 키워드 사용시 주의점(메모리 누수), Final 키워드 선택 원칙

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

#### Q1. Stack 영역은 뭐야?
```
Stack 영역은 '메서드 호출 정보'가 담긴 '스택 프레임'을 후입선출(LIFO) 구조로 쌓는 영역이다. 쓰레드별로 하나씩 생성되며 쓰레드의 메서드 흐름을
관리하기 위한 영역으로 보면 된다.
```

<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/659e5789-cf53-4611-b275-5ce6b6cc5de6" width="500">

* [그림2]. JVM의 스택 메모리 영역

```
사실 우리는 이 스택 정보를 많이 접해봤는데 바로 '스택 트레이스'가 그 예이다. 우리가 에러를 냈을 때 아래와 같은 해당 스레드의 흐름 정보를 볼 수
있는데 이건 사실 스택 영역에 적재된 내용들을 pop 해서 보여주는 것이다.
```

<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/20a183c8-7a83-43f8-a3d5-e49ba3d8c4c8" width="500">
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/ef17a3e8-9f21-4384-9c97-014b6bc76e97" width="500">

* [그림3]. Case1: StackTrace

```
이렇게 스레드 별로 메서드 호출 흐름을 담는 역할을 하다보니 "메소드 호출이 굉장히 많이 일어나고 깊게 일어나는 경우", 예컨대 재귀함수를 잘못 쓰는 경우
등의 문제가 발생했을 때 그 유명한 'StackOverflow' 라는 이름의 에러가 뜬다. 지금 생각해보면 이것도 이 스택 영역이 꽉 찼다는 의미로 받아들이면 될 거 같다.
```

<br>


#### Q2. PC 영역은 뭐야?
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/8c94325a-b092-482a-9bb9-def0e702fc4f" width="500">

* [그림4]. JVM의 PC 영역

```
PC 영역은 Stack 영역의 메서드 흐름이 어디까지 진행되고 있는지 정보를 기록하기 위해 필요한 영역이다. 결국 하나의 작업을 직렬 처리하는게 아니기
때문에 작업이 끝날 떄 까지 CPU 점유하지 않을 수 있다. 그래서 해당 작업이 어디까지 진행됐는지 기록 한다.
```

<br>

#### Q3. Native Method Stack 영역은 뭐야?
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/9905fc49-174d-405d-8997-e26a15a627db" width="500">
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/80ffac9c-0a90-4db7-9baa-62d3a32dd344" width="500">

* [그림5]. Native Method 영역에 대해서

```
자바 프로그램도 내부적으로 C로 구현된 메서드들을 사용하는데 'native' 키워드를 통해 JNI를 호출해 실제 네이티브 라이브러리에 있는 c 코드를 호출
할 수 있다. 이 때도 마찬가지로 작업 흐름이 쌓일텐데 이를 저장하는 영역이다.
```

<br>

#### Q4. Method 영역은 뭐야?
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/b925fcc7-9214-47e8-8565-9814d190239b" width="500">



* [그림6]. Method 영역에 대해서

```
Method 영역은 Class나 method의 '메타 데이터'들을 저장하는 공간이다. 사실 java 8 이전에는 Heap 영역 하위에 Permanent Generation 영역에 구현돼 있었으나
java 8 버전부터 metaspace라는 이름의 영역으로 분리되어 나왔다. 이 때부터 더 이상 jvm의 java heap이 아닌 native heap 영역에 속하게 된다. 덕분에 jvm 메
모리에 독립적으로 사용할 수 있게 된 셈이다.
```




<br>

#### Q5. Heap 영역은 뭐야?
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/b925fcc7-9214-47e8-8565-9814d190239b" width="500">
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/ea8004ce-494f-444c-8794-514a3a523900" width="500">

* [그림7]. Heap 영역에 대해서


```
Heap 영역은 우리가 객체나 배열을 생성할 때 동적 정보를 저장하기 위한 가상 영역이다. Minor, Major, Full GC의 영향을 받는 영역이다.
```

<br> 

----

## 2. 자바 Garbage Collector에 대해서

```
GC를 간단히 설명하면 안쓰는 객체를 회수하는 녀석이다. 
```

> [@고민해볼 관점]
> 
> GC 각 알고리즘의 차이점, Garbage Collection 모니터링과 메모리 설정시 고려 사항, GC 모니터링 툴(jstat, visualvm, HPJMeter, YourKit),
> GC 튜닝(설정 조정이 minot/major GC에 미치는 영향), 


####  "쓰레기 청소부"

```
오늘 이해해볼 내용은

1. GC의 청소 대상
2. GC의 청소 방식
3. GC의 알고리즘엔 어떤게 있는지
4. Garbage Collection을 모니터링 하는 방법
5. 메모리 설정(?)시 고려사항: 정확히 어떤 메모리 설정?
6. GC 모니터링 툴들에 대해서 리뷰
7. GC를 튜닝 한다는 것 (설정 조정이 minor/major Gc에 미치는 영향)
8. Heap에 대한 GC와 method에 대한 GC의 차이
```

<br>

#### Q1. GC의 청소 대상에 대해서


- 청소 대상 그림 -

* [그림8]. Heap 영역에 대해서

```
GC는 어떤 객체들을 청소할까? 아무거나 쓸어담는다면 GC(Garbage Collector)가 아니라 (Object Destoryer)라고 불리게 됐을 것이다.
나름대로 청소를 하는 규칙이 있다는데 그 기준이 어떤건지 오늘 다뤄보려고 한다.
```

> [@청소 기준]
>
> - 연결이 끊긴 객체 (native method stack, stack, method로 부터 더 이상 참조가 없는 경우)
> - '참조 없는 상태를 파악하는 방법은, RootSpace부터 그래프 순회를 통해 연결된 객체들을 마킹하고 마크가 없는 객체들을 제거한다.

<img src ="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/d38c224f-0b88-406b-be81-016aa5ed58b3" width="400">
<img src ="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/da64bc28-9810-439a-bc72-9498cd29849e" width="600">

* [그림9]. 그래프 탐색과 마킹


<br>



