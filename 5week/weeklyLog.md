
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

### 1. Java Synchronized Collection과 Concurrent Collection을 비교하시오

> 둘 다 thread safety를 지원하기 위해 컬렉션 클래스인데 `데이터를 보호하는 범위와 방식`에 차이가 있습니다. synchronized Collection은 전체 컬렉션을 잠구고
> Concurrent Collection은 특정 부분에만 락 프리를 적용하는 방식으로 thread safety를 보장합니다. 그래서 synchronized Collcetion보다 Concurreent Collection이
> 대체로 성능이 더 좋은 편입니다.

```
(Q1. 근데 무조건 더 좋나? 실제로 어떤 상황에서 더 좋은지 비교 해보기),
(Q2. 세그먼트에 대한 명확한 정의 및 실체 찾아보기)
(Q3. CAS: 기대하는 연산 결과와 같거나 하드웨어 레벨에서 스레드 침범이 일어나지 않음을 확인받은 상태에서만 변수의 값을 업데이트, 라는 설명 맞는지 다시 검토)
```

##### 질문 1-1. 어떤 상황에서 Synchronized Collection을 사용하는 것이 좋고, 어떤 상황에서는 Concurrent Collection을 사용하는 것이 좋나요?

> (1) `synchronized collction`은 `동시에 많은 스레드가 작업하지 않거나 복잡도가 낮은 간단한 동기화 작업에 적합`합니다.
> 
> (2) 반면 `Concurrent Collection`은 `고성능이 필요하거나 많은 스레드가 데이터에 동시에 접근해야 하는 상황`에서 더 나은 선택입니다.

> 근데 사실 이게 요청 받아 줄 때 까지 `쓰레드 가지고 있는 상태로 요청 계속 반복해서 쏘냐`, `CPU 자원 돌려주고 락 받을 때 까지 기다리냐` 차이여서
> 이런 부분 고려해서 선택하면 좋을 거 같습니다.



##### 질문 1-2. Concurrent Collection이 특정 부분만 잠그는 방식으로 동시성을 관리한다고 했는데, 이게 구체적으로 어떤 원리로 작동하나요?
> 자바 8 이전에는 `맵을 여러 부분으로 나누어 독립적으로 잠구는 방식`을 사용하다가 8버전 부터는 CAS 연산이라는 걸 이용해서 `락 없이 thread safety를 준수`하는 방식을
> 사용합니다.


##### 질문 1-3. "Synchronized Collection과 Concurrent Collection 모두 스레드 안전하다고 하는데, 그럼 둘 사이에 안전성 면에서 차이가 있나요?"
> 제가 느끼기엔 큰 차이는 없고 다만 Concurrent collection이 Compute 같은 복합 연산을 위한 메서드 정도를 더 잘 지원해주는 것 정도로 이해했습니다.



##### 질문 1-4. "Synchronized Collection과 Concurrent Collection 외에 다른 동시성 컬렉션 기술이 있나요?"
> 뭐 .. `BlockingQueue`나 `CopyOnWrite ~` 같은 것들이 있습니다.


##### 질문 1-5. CopyOnWriteArrayList는 뭔데요?
> ... 걍 복사본에 쓰게 하는겨

<br>



### 2. HashMap 동작원리와 내부구조에 대해 설명하시오 (키워드: equals/hashCode와의 상관관계 및 성능)
[참고1.](https://www.baeldung.com/java-equals-hashcode-contracts) , 


##### 질문 2-1. HashMap의 동작 원리와 내부 구조에 대해서 .. 

> 우리가 배열을 이용할 때 index 같은 걸 사용해서 쉽게 데이터에 접근할 수 있잖아요. 근데 이제 `index는 사용하기에 직관적이지 않으니까 이걸 key라는 걸로 대체`해서
> 데이터 관리를 보다 쉽고 유연하게 지원해주기 위한 자료구조가 HashMap 입니다!

```
내부적으로는 key로 넘어온 객체의 hashCode를 이용해 hash key를 받아서 인덱스로 변환하는데 이 주소를 가지고 값을 저장하거나 검색합니다. 이 과정에서 '여러개의 키가 같은 인덱스를 사용하는 경우가
생길 수 있는데 이를 '충돌'이라고 표현하고 '체이닝'과 '개방 주소법'등의 방법을 통해 해결할 수 있습니다. HashMap은 이런 충돌 해결에 대해 '체이닝'이라는 기법을 적용합니다. 이를 위해 java8 이전에는'연결 리스트'
를 활용했고 그 이후부터는 '레드 블랙 트리'를 활용합니다.
```
##### @ TODO: 여기 질문에 맞게, 동작 원리와 내부 구조 설명하는데 집중하기. ***************************************

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

### 3. JSP의 처리 과정에 대해 설명하시오 (키워드: 구성요소, servlet, request/response)


> JSP의 처리 과정은 서버로 어떤 페이지를 달라는 `요청이 들어오면` web.xml 라던지, 스프링의 경우 컨트롤러 애노테이션이 붙은 메서드의 응답으로 해당 `jsp의 실제경로(물리 경로)를 지정`해 내려주면 뷰리졸버 같은 것들이
> 해당 jsp를 호출하고 `해당 내용이 톰캣에 의해 서블릿으로 변환`되고 이 `서블릿 내용이 톰캣에 의해 html로 파싱`되서 `응답으로 내려갑니다`.


###### 이 전체 과정은 마치 요리를 하는 것과 비슷하다. 
```
(1) 요리 주문이 들어오면(웹 페이지 요청),
(2) 요리사(서버)는 레시피(웹.xml 또는 스프링 컨트롤러)를 확인한다. 
(3) 그 다음에 요리사는 재료(JSP 파일)를 준비하고,
(4) 요리(서블릿 변환)를 한 후에,
(5) 마지막으로 음식(HTML 페이지)을 손님(사용자)에게 제공한다.
```


##### Q1. jsp요? 근데 굳이 왜 jsp를 쓸까요? 서블릿을 그냥 쓰면 안되나요?
```
한마디로 서블릿은 자바 코드여서 html을 다루는데 적합하지 않아서, html 형식 베이스에 자바 코드를 삽입하는 방식인 jsp를 사용합니다. 그리고 이렇게 표현계층과 비즈니스 로직을 분리하면 개발도 편해집니다.
```
> 사실 `여기서 말하는 서블릿`은 Servlet 이라는 클래스를 상속받는 `HTTP 요청에 대한 처리를 하기 위한 클래스`를 말하는건데요. 이게 `사실 그냥 자바 코드`여서요! html 작성을 하려고 해도 저희가 System.out.print()같은 거 하듯이 writer객체에 일일이 
> 써줘야하는데 이게 `html을 다루는 방법으로는 직관적이지 못한 편`이어서, 그렇다고 자바 레벨에서 뭔가 더 개선하는것도 사실 자바 코드위에서 일어나는 일이어서 한계가 있어서요. 아예 html을 베이스로 하되 거기에 동적으로 필요한 값들에 대해서 java 코드를
> 삽입할 수 있도록 해서 html개발을 보다 직관적으로 하기 위해 jsp를 씁니다.
>
> `개발자는 복잡한 서블릿 코드를 직접 관리하는 대신, JSP를 통해 비즈니스 로직과 프레젠테이션 레이어를 쉽게 분리할 수 있게 됐습니다.`

<br>



##### Q2. JSP와 Servlet의 차이를 좀 봅시다.

> ##### example(1) servlet
```
...
public class GreetingServlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        String name = request.getParameter("name");
        if (name == null) name = "World";
        out.println("<html>");
        out.println("<head><title>Greeting</title></head>");
        out.println("<body>");
        out.println("<h1>Hello, " + name + "!</h1>");
        out.println("</body>");
        out.println("</html>");
    }
}
```

> ##### example(2) jsp
```
...
<html>
<head><title>Greeting</title></head>
<body>
    <% String name = request.getParameter("name");
       if (name == null) name = "World";
    %>
    <h1>Hello, <%= name %>!</h1>
</body>
</html>
```

<br>
<br>


### 4. MVC패턴에 대해 설명하시오 (키워드: 모델, 컨트롤러, 뷰, MVC1 vs MVC2 vs SpringMVC 비교)
> #####  
```

```

##### 질문 4-1. 그래서 저건 뭔데요?
```

```

<br>



### 5. 쿠키와 세션에 대해 설명하시오 (키워드: 사용 이유, 장단점, 차이점)

```

```

##### 질문 5-1. 그래서 저건 뭔데요?
```

```

<br>


### 6. http methods(POST, GET, PUT, PATCH, DELETE, etc)와 http status code(1xx, 2xx, 3xx, 4xx, 5xx)대해 설명하시오
```

```

##### 질문 6-1. 그래서 저건 뭔데요?
```

```

<br>





..
