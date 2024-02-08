
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

##### Q1. 어떤 상황에서 Synchronized Collection을 사용하는 것이 좋고, 어떤 상황에서는 Concurrent Collection을 사용하는 것이 좋나요?

> (1) `synchronized collction`은 `동시에 많은 스레드가 작업하지 않거나 복잡도가 낮은 간단한 동기화 작업에 적합`합니다.
> 
> (2) 반면 `Concurrent Collection`은 `고성능이 필요하거나 많은 스레드가 데이터에 동시에 접근해야 하는 상황`에서 더 나은 선택입니다.

> 근데 사실 이게 요청 받아 줄 때 까지 `쓰레드 가지고 있는 상태로 요청 계속 반복해서 쏘냐`, `CPU 자원 돌려주고 락 받을 때 까지 기다리냐` 차이여서
> 이런 부분 고려해서 선택하면 좋을 거 같습니다.



##### Q2. Concurrent Collection이 특정 부분만 잠그는 방식으로 동시성을 관리한다고 했는데, 이게 구체적으로 어떤 원리로 작동하나요?
> 자바 8 이전에는 `맵을 여러 부분으로 나누어 독립적으로 잠구는 방식`을 사용하다가 8버전 부터는 CAS 연산이라는 걸 이용해서 `락 없이 thread safety를 준수`하는 방식을
> 사용합니다.


##### Q3. "Synchronized Collection과 Concurrent Collection 모두 스레드 안전하다고 하는데, 그럼 둘 사이에 안전성 면에서 차이가 있나요?"
> 제가 느끼기엔 큰 차이는 없고 다만 Concurrent collection이 Compute 같은 복합 연산을 위한 메서드 정도를 더 잘 지원해주는 것 정도로 이해했습니다.



##### Q4. "Synchronized Collection과 Concurrent Collection 외에 다른 동시성 컬렉션 기술이 있나요?"
> 뭐 .. `BlockingQueue`나 `CopyOnWrite ~` 같은 것들이 있습니다.


##### Q5. CopyOnWriteArrayList는 뭔데요?
> ... 걍 복사본에 쓰게 하는겨

<br>



### 2. HashMap 동작원리와 내부구조에 대해 설명하시오 (키워드: equals/hashCode와의 상관관계 및 성능)
[참고1.](https://www.baeldung.com/java-equals-hashcode-contracts) , 


##### Q1. HashMap의 동작 원리와 내부 구조에 대해서 .. 

> 우리가 배열을 이용할 때 index 같은 걸 사용해서 쉽게 데이터에 접근할 수 있잖아요. 근데 이제 `index는 사용하기에 직관적이지 않으니까 이걸 key라는 걸로 대체`해서
> 데이터 관리를 보다 쉽고 유연하게 지원해주기 위한 자료구조가 HashMap 입니다! key로 넘어온 객체를 hash함수에 넣어서 해쉬 값을 추출한 걸 가지고 값을 실제 저장할 인덱스를 추출하는데 이 때 해당 객체의 식별 값 역할을 하는 hashCode가
> 필요합니다! 그래서 결국 실제 주소를 추출한 뒤에는 이걸 이용해 값을 저장하거나 검색하는데 이 과정에서 '여러개의 키가 같은 인덱스를 사용하는 경우가 생길 수 있습니다. 이를
> '충돌'이라고 표현하고 '체이닝'과 '개방 주소법'등의 방법을 통해 해결할 수 있습니다. HashMap은 이런 충돌 해결에 대해 '체이닝'이라는 기법을 적용합니다. 이를 위해 java8 이
> 전에는'연결 리스트'를 활용했고 그 이후부터는 '레드 블랙 트리'를 활용합니다. 이런 것들은 버킷으로 표현됩니다.


##### Q2. HashMap의 성능은 어떤 것들에 영향을 받을까요?
> HashMap의 용량이나 부하 계수에 영향을 받을 수 있을 거 같습니다. 부하 계수란건 버킷 수에 비해 노드가 얼마나 존재하냐에 대한 수치인데 결국 버킷 수는 적은데 노드가 많다는 건
> '충돌'이 많다는 걸 의미하고 이는 특정 값을 찾는데 드는 시간이 길어졌다는 걸 의미해서요! 이런 척도들이 있고 해쉬맵에선 사실 재해싱을 언제 할건지 판단을 위한 부하 계수 한계치나
> 버킷안의 요소들의 용량등을 설정해줄 수 있습니다.


##### Q3. 실제 해시맵을 구현한다면 어떤 것들을 만들거세요?
> HashTable 클래스를 만들고 그 안에서 아요할 노드를 만들건데 그 안에는 노드에 필요한 hash값과 key값, value, 그다음 다음 노드를 가르킬 next가 필요할 거 같습니다.
> 여기서 hash값은 현재 key를 hash 함수에 넣어 얻은 hash value를 의미합니다.


##### 질문 2-1a. 레드블랙 트리가 뭔가요 ..?
```
걍 별 거 아니고 이진 검색 트리의 한 형태입니다!
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


##### Q1. 근데 굳이 왜 jsp를 쓸까요? 서블릿을 그냥 쓰면 안되나요?
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
> #####  JSP 파일만 쓸 경우 하나에 비즈니스 로직, 랜더링할 html 코드, 심지어 레포지토리에 대한 내용까지 전부 들어가버리게 되어 코드가 복잡해집니다. `그래서 MVC가 필요해집니다`

```
[History]
(1-A) 정적인 웹을 동적으로 바꿔주기 위해 Servlet을 썼다.
(1-B) 근데 Servlet처럼 자바 코드로 html 쓰기가 말도 안되게 힘드네?

(2-A) JSP라는 템플릿 엔진의 등장. 이제 html 동적으로 랜더링하기 좀 편해짐
(2-B) 근데 JSP파일 하나에 뷰, 비즈니스 로직, 레포지토리까지 다 우겨 넣으니까 유지보수의 문제 발생

Answer. 그래서 나온게 바로 ... MVC 패턴 !
```

![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/435a1638-049f-48e7-ac72-f59d2dcc1067)
##### [그림]. MVC 패턴의 2가지 구조

```
[구조1]
1. 컨트롤러에 비즈니스 로직까지 담아서
2. 모델에 데이터 전달하고
3. 뷰에서 참조한 뒤에
4. 이를 Response 해주는 것과

[구조2]
1. 컨트롤러와 서비스(비즈니스)로직을 분리한 후에
2. 모델에 데이터 전달하고
3. 뷰에서 참조해서
4. 이를 Response 해주는 것
```
##### > 일단 Servlet과 JSP를 이용해 위 MVC2 구조를 따라해보면 아래와 같다.
```
// Servlet part
@WebServlet(name = "MvcMemberListServlet", urlPatterns = "/servlet-mvc/members")

public class MvcMemberListServlet extends HttpServlet {
    MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        List<Member> memberList = memberRepository.findAll();
        req.setAttribute("members", memberList);

        String viewPath = "/WEB-INF/views/members.jsp";
        RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
        dispatcher.forward(req, resp);
    }
}
```
> 위 코드를 보면 알겠지만 서블릿 관련 내용 등록해주는 부분이나 distpatcher에 req나 resp를 forward해주는 부분이 계속 중복됩니다.
> 이런 불편함을 해소하기 위해서 "Front controller" 패턴 이라는 걸 적용해서 Front Controller를 제외한 나머지 컨트롤러들은 servlet을 사용치 않아도 되게 구현했다.

![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/fde071af-d40e-41e6-b43f-ce09d48be52f)


> 사실 SPRING MVC는 다양한 문제들을 해결하면서 발전한 부분이어서 한번에 다 설명 드리긴 어렵지만 결국 Dispatcher Servlet이라는 Front Controller의 역할을 하는 녀석이
> HTTP 요청에 맞는 핸들러를 조회하고 해당 핸들러를 처리할 수 있는 어댑터를 찾아서 해당 핸들러를 호출하고, 그 핸들러에서 처리한 모델과 뷰 정보를 내려주면 view Resolver
> 를 이용해 해당 View 를 찾은 다음에 랜더링해서 HTML을 내려주는 구조입니다.

##### >example code
```
<%@ page import="java.util.List" %>
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
 MemberRepository memberRepository = MemberRepository.getInstance();
 List<Member> members = memberRepository.findAll();
%>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <a href="/index.html">메인</a>
        <table>
             <thead>
             <th>id</th>
             <th>username</th>
             <th>age</th>
             </thead>
             <tbody>
            <%
             for (Member member : members) {
                 out.write(" <tr>");
                 out.write(" <td>" + member.getId() + "</td>");
                 out.write(" <td>" + member.getUsername() + "</td>");
                 out.write(" <td>" + member.getAge() + "</td>");
                 out.write(" </tr>");
             }
            %>
             </tbody>
        </table>
    </body>
</html>
```
<br>



### 5. 쿠키와 세션에 대해 설명하시오 (키워드: 사용 이유, 장단점, 차이점)

> 사실 이게 HTTP 때문에 필요한 건데요, HTTP는 무상태성을 가지기 때문에 사용자 정보를 연속적으로 가져갈 수가 없습니다. 그래서 쿠키라는 걸 사용하는데요, 이 쿠키는 서버에서
> 생성되서 사용자 브라우저에 저장되는 파일인데요, 이 쿠키를 받은 도메인에 대해 요청을 보낼 떄 같이 보낼도록 설정할 수 있어 사용자의 작업에 상태를 부여할 수 있습니다.
> 세션은 사용자의 정보를 세션이란 개념으로 서버에서 저장하는데, 저는 이게 JVM의 인메모리에 저장될 거라고 생각합니다. 어쨌든 각 세션에 대한 ID를 쿠키를 통해 사용자에게 전달하면
> 사용자는 매 요청마다 자신의 세션 ID를 서버로 전달하게 되고 서버는 이를 통해 상태값을 관리할 수 있게 됩니다.

++ 쿠키는 SameStie, Http Only 등의 옵션을 통해 보안 처리를 고려하면 좋음

##### Q1. 그래서 언제 쓰는데요?
```
서버에 부하를 많이 주는지나 보안적인 면을 두고 고민할 거 같습니다. 트래킹 해도 괜찮은 사용자 정보같은 것들은 쿠키를 이용할 거 같고
사용자의 로그인 정보 같은 것들은 세션을 이용할 거 같습니다.
```

<br>


### 6. http methods(POST, GET, PUT, PATCH, DELETE, etc)와 http status code(1xx, 2xx, 3xx, 4xx, 5xx)대해 설명하시오
```
HTTP Method는 요청이 어떤 목적인지를 대략적으로 나타내기 위해 사용하는 구분입니다. POST는 신규 리소스를 생성하거나 어떤 작업을 수행할 때 라던지
GET은 조회 작업, PUT은 리소스를 대체 할 때, PATCH은 리소스를 부분적으로 업데이트 할 때, DELETE는 리소스를 삭제 할 때 등의 목적을 표현할 떄 사용합니다.

Http status는 해당 요청의 상태를 표현하기 위해 사용하는데 100번대는 '요청이 수신되어 처리중' 상태를 나타내고
200번대는 정상 처리, 300번대는 요청 완료했을 때 추가적인 행동이 필요하다는 의미입니다. 400은 클라이언트 측 오류, 500은 서버측 오류를 표현하는 코드입니다.

```

##### 질문 6-1. 그래서 저건 뭔데요?
```

```

<br>





..
