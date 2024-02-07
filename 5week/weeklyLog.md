
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

##### 질문 1-1. -

```
-
```

<br>



#### 2. HashMap 동작원리와 내부구조에 대해 설명하시오 (키워드: equals/hashCode와의 상관관계 및 성능)
[참고1.](https://www.baeldung.com/java-equals-hashcode-contracts) , 


##### 질문 2-1. HashMap의 동작 원리와 내부 구조에 대해서 .. 

> 우리가 배열을 이용할 때 index 같은 걸 사용해서 쉽게 데이터에 접근할 수 있잖아요. 근데 이제 `index는 사용하기에 직관적이지 않으니까 이걸 key라는 걸로 대체`해서
> 데이터 관리를 보다 쉽고 유연하게 지원해주기 위한 자료구조가 HashMap 입니다!

```
내부적으로는 key로 넘어온 객체의 hashCode를 이용해 hash key를 받아서 인덱스로 변환하는데 이 주소를 가지고 값을 저장하거나 검색합니다. 이 과정에서 '여러개의 키가 같은 인덱스를 사용하는 경우가
생길 수 있는데 이를 '충돌'이라고 표현하고 '체이닝'과 '개방 주소법'등의 방법을 통해 해결할 수 있습니다. HashMap은 이런 충돌 해결에 대해 '체이닝'이라는 기법을 적용합니다. 이를 위해 java8 이전에는'연결 리스트'
를 활용했고 그 이후부터는 '레드 블랙 트리'를 활용합니다.
```

##### 질문 2-1a. 레드블랙 트리가 뭔가요 ..?
```
이진 검색 트리의 한 형태입니다!
```

##### 질문 2-2. equals()와 hashCode()의 상관관계에 대해서
```
사실 equals()와 hashCode()사이에는 직접적인 의존관계는 없습니다. 다만 둘 다 객체의 동등성이나 동일성 비교를 위해 필요한 개념이다보니 둘 중 하나를 변경했다면 나머지 하나도 그 기준에 맞춰 변경해주어야 한다는
'계약 관계'가 있습니다. 예컨대 hashMap 같은 곳에서 put을 할 때는 내부적으로 key로 넘어온 객체의 hashCode를 이용하는데 이 떄 hashCode의 기준이 equals와 같지 않다면 의도치 않은 결과가 발생하게 됩니다.
```



<br>

#### 3. JSP의 처리 과정에 대해 설명하시오 (키워드: 구성요소, servlet, request/response)

```
옛날에 웹 페이지를 동적으로 렌더링하기 위해 Servlet을 사용했었지. 여기서 개발자는 HttpServletResponse 객체를 통해 직접 HTML 요소를 문자열 형태로 작성해야 했어. 이 방식은 직관적이지 않고 유지 보수하기 어려운 문제가 있었지. 이러한 어려움을 해결하기 위해 JSP(Java Server Pages)가 등장했어. JSP는 HTML 코드 안에 자바 코드를 삽입할 수 있는 파일 형태를 제공해. 이를 통해 개발자는 웹 페이지를 더 직관적으로 동적으로 생성할 수 있게 됐지.

개발자가 JSP 파일을 작성하고 서버에 요청하면, 톰캣 같은 서블릿 컨테이너는 이 JSP 파일을 자동으로 Java Servlet으로 변환해. 이 과정은 뒷단에서 자동으로 일어나며, 변환된 Servlet은 서버 상에서 실행되어 클라이언트의 요청을 처리한 뒤, 최종적으로 HTML 형식의 응답을 생성해내. 이렇게 생성된 응답은 클라이언트에게 전송되고, 클라이언트의 브라우저는 이 응답을 받아 웹 페이지를 렌더링하게 되지.

이 방식 덕분에 웹 개발 과정이 더욱 효율적이고 직관적으로 변했어. 개발자는 복잡한 서블릿 코드를 직접 관리하는 대신, JSP를 통해 비즈니스 로직과 프레젠테이션 레이어를 쉽게 분리할 수 있게 됐지.
```

##### 질문 2-1. jsp의 서블렛 변환 과정 주체 (톰캣인가 내부 로직인가 ..)

jsp로 넘겨줄 모델 값을 세팅하고 디스패처 같은 클래스에게 넘겨주면 servlet로 자동 변환
```
protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
    // 데이터를 request에 추가
    request.setAttribute("key", "value");

    // RequestDispatcher를 사용하여 JSP 페이지로 요청 전달
    RequestDispatcher dispatcher = request.getRequestDispatcher("/WEB-INF/example.jsp");
    dispatcher.forward(request, response);
}
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
