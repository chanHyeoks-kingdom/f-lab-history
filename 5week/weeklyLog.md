
# WEEK 5

```
[이 주의 과제]
1. Java Synchronized Collection과 Concurrent Collection을 비교하시오
2. HashMap 동작원리와 내부구조에 대해 설명하시오 (키워드: equals/hashCode와의 상관관계 및 성능)
3. JSP의 처리 과정에 대해 설명하시오 (키워드: 구성요소, servlet, request/response)
4. MVC패턴에 대해 설명하시오 (키워드: 모델, 컨트롤러, 뷰, MVC1 vs MVC2 vs SpringMVC 비교)
5. 쿠키와 세션에 대해 설명하시오 (키워드: 사용 이유, 장단점, 차이점)
6. http methods(POST, GET, PUT, PATCH, DELETE, etc)와 http status code(1xx, 2xx, 3xx, 4xx, 5xx)대해 설명하시오
```

-----


## [자문자답]

#### 1. Java Synchronized Collection과 Concurrent Collection을 비교하시오
```
-
```

##### 질문 1-1. 그래서 이건 뭔데요?
```

```

<br>



#### 2. HashMap 동작원리와 내부구조에 대해 설명하시오 (키워드: equals/hashCode와의 상관관계 및 성능)

##### 질문 2-1. HashMap의 동작 원리와 내부 구조에 대해서 .. 

> 우리가 배열을 이용할 때 index 같은 걸 사용해서 쉽게 데이터에 접근할 수 있잖아요. 근데 이제 `index는 사용하기에 직관적이지 않으니까 이걸 key라는 걸로 대체`해서
> 데이터 관리를 보다 쉽고 유연하게 지원해주기 위한 자료구조가 HashMap 입니다!

```
내부적으로는 key를 받으면 hash function을 이용해서 인덱스로 변환해서 값을 저장하거나 검색하는데 사용합니다. 이 과정에서 '여러개의 키가 같은 인덱스를 사용하는 경우가
생길 수 있는데 이를 '충돌'이라고 표현하고 '체이닝'과 '개방 주소법'등의 방법을 통해 해결할 수 있습니다. 이런 충돌 해결에 '연결 리스트'나 '레드 블랙 트리'를 활용합니다.
```


##### 질문 2-2. equals()와 hashCode()의 상관관계에 대해서
```

```

<br>

#### 3. JSP의 처리 과정에 대해 설명하시오 (키워드: 구성요소, servlet, request/response)

```
JVM은 메모리를 용도에 맞게 분리해 다양핱 성능상의 이점과 보안상의 이점을 가져가는데 대표적인 가상 메모리 영역의 예시는 "Method", "Heap", "Stack" 영역이 있습니다.

```

##### 질문 2-1. 그래서 저건 뭔데요?
```

```

<br>


#### 4. MVC패턴에 대해 설명하시오 (키워드: 모델, 컨트롤러, 뷰, MVC1 vs MVC2 vs SpringMVC 비교)

```
```

##### 질문 4-1. 그래서 저건 뭔데요?
```

```

<br>



#### 5. 쿠키와 세션에 대해 설명하시오 (키워드: 사용 이유, 장단점, 차이점)

```

```

##### 질문 5-1. 그래서 저건 뭔데요?
```

```

<br>


#### 6. http methods(POST, GET, PUT, PATCH, DELETE, etc)와 http status code(1xx, 2xx, 3xx, 4xx, 5xx)대해 설명하시오
```

```

##### 질문 6-1. 그래서 저건 뭔데요?
```

```

<br>





..
