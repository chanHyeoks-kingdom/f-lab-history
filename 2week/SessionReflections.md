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
---

<br>

--- 

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
<img src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/ffb47350-a2de-4544-937e-174a0a587929" width="500">

* [그림6]. Method 영역에 대해서

```
Method 영역은 
```







```
contents 2
```
<br>

## 2. subject 2 ..

```
..
```

####  "객체지향은 복잡한 시스템을 이해하기 쉬운 형태로 만들기 위해서 써야 한다."

<br>

```
