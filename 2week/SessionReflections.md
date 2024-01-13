

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
contents
```

#### "대부분의 소프트웨어는 필연적으로 변경되는데 만약 서비스가 복잡해져 이해하기 어려운 상태가 된다면 소프트웨어는 지속적으로 변경될 수 없다."

![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/3e6df0d9-4759-4dea-aa2e-7f4492db8451)


<br>

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
