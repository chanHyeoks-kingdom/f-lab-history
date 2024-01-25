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
자바에선 여러 자료구조를 손쉽게 사용하기 위해 Collections 라는 프레임워크를 사용할 수 있는데요! 여기서 지원하는 자료구조 중에서 'Value'만을 이용해 데이터를 관리하는 자료구조의 컨셉이 되게 Collection라는
인터페이스 입니다. 자바에선 리스트, Set, Queue로 확장했는데 이 내용에 대해 좀 더 구체적으로 설명하면 좋을까요? 아니면 다른 궁금한 부분이 있으실까요?
```


#### 질문 1-1. Map이랑은 왜 분리된거예요?
```
아무래도 데이터 관리를 위한 스펙이 다르다 보니 그런 거 같습니다. Collections 라이브러리는 Value만 이용해 데이터를 관리하는 반면에 Map은 Key와 Value라는 2가지 값들을 사용해야 해서 하나로 합치기 어려웠던
거 같습니다. 사실 내부적으론 기능 단위로 봤을 때 값 삽입, 삭제, 조회 같은 역할을 해서 큰 차이가 없긴 한데 좀 특이한 차이라고 하면 Collection은 iterable이라는 인터페이스를 확장해서 for-each를 사용할 수
있다 정도의 차이가 있는 거 같습니다.
```

##### 질문 1-2. 리스트부터 설명 해주실래요? 왜 쓰는거에요?
```
네! 리스트는 동적 배열 기반의 ArrayList와 이걸 thread-safety하게 구현한 Vector, 그리고 이 Vector을 상속받아서 LIFO를 제공토록 구현된 STACK 등이 있습니다.
ArrayList는 인덱스를 사용할 수 있는 환경에서 여러 위치의 요소에 대한 빠른 조회등을 할 때 주로 사용하면 좋고 Vector는 이 ArrayList에 threadSafety이 필요할 때 사용하면 되는데 사실 성능 이슈가 있어서요!
자주 쓰는 곳에만 사용하고 되도록이면 쓰기작업과 읽기작업의 빈도를 보고 concurrent collection의 CopyOnWriteArrayList나 Collections의 synchronizedList를 이용해 처리해주는게 일반적으론 더 좋습니다.
```

##### 질문 1-3. CopyOnWriteArrayList랑 synchronizedList가 뭐죠?
```
둘 다 ThreadSafety를 보장하기 위해 지원하는 메서드들인데요!

자바에서 자체적으로 컬렉션이 한 스레드에 의해서 순회할 때 접근할 경우 ConcurrentModificationException라는 예외가 던져지기도 하고 아예 동시에 수정과 읽기가 일어났을 때 계획과 다른 값을 읽게할 수 있는데요!
이 떄 쓸 수 있는게 쓰기 작업이 발생할 때 마다 복사본을 만들어 읽기 작업에 대한 안전성을 보장하는 CopyOnWriteArrayList이나 사용중인 자원에 대한 접근을 애초에 막아버리는 synchronizedList를 사용할 수 있습니다.
이 두 가지 방법은 Vector등을 사용하는 것 보다 나은 성능을 가질 수 있습니다. 최근에는 리스트의 thread-safety를 보장하기 위해 주로 사용한다고 이해했습니다.
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
