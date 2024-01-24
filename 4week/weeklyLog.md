# WEEK 4

```
[이 주의 과제]
1. Java Collection에 대해 설명 하시오 (키워드: Set, Map, List, Queue, Stack, etc)
2. Java Synchronized Collection과 Concurrent Collection을 비교하시오
3. HashMap 동작원리와 내부구조에 대해 설명하시오 (키워드: equals/hashCode와의 상관관계 및 성능)
4. JSP의 처리 과정에 대해 설명하시오 (키워드: 구성요소, servlet, request/response)
5. MVC패턴에 대해 설명하시오 (키워드: 모델, 컨트롤러, 뷰, MVC1 vs MVC2 vs SpringMVC 비교)
```


-----


## [경계성 질문]

#### 질문 1. Java Collections이 뭔가요?
```
자바에선 여러 자료구조를 손쉽게 사용하기 위해 Collections 라는 프레임워크를 사용할 수 있는데 여기서 지원하는 자료구조 중에서 'Value'만을 다루기 위한 여러 자료구조의 컨셉이 되게 Collection이라는
인터페이스 입니다. 주로 '목록형 데이터 집합'인 리스트, Set, Queue로 확장할 수 있는데 이 내용에 대해 좀 더 구체적으로 설명하면 좋을까요? 아니면 다른 궁금한 부분이 있으실까요?
```

##### 질문 1-1. 리스트부터 설명 해주실래요? 왜 쓰는거에요?
```
네! 리스트는 

리스트는 순서가 중요한 자료구조들이 들어가는데요 예를 들어 index 조회 주로 이용하는 ArrayList나 거기에 Thread Safety를 적용한 Vector 그리고 그걸 상속받은 후입 선출 구조의 Stack등이 있습니다.

```

##### 질문 1-2. ArrayList랑 LinkedList의 차이는 뭔가요?
```
ArrayList는 동적 배열이고, LinkedList는 
```


<br>



#### 질문 2. Java Synchronized Collection과 Concurrent collection은 무슨 차이에요?
```
JVM은 메모리를 용도에 맞게 분리해 다양핱 성능상의 이점과 보안상의 이점을 가져가는데 대표적인 가상 메모리 영역의 예시는 "Method", "Heap", "Stack" 영역이 있습니다.

```

##### 질문 2-1. 그래서 저건 뭔데요?
```

```

<br>


..
