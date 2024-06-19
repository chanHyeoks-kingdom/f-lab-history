```
금주 한 것

1. ..

```


```
[금주 PLAN]
[일]: 9시간 - PJT 투자 (공통 모듈, 결제 도메인 개발, 결제 도메인에 공통 모듈 작성), DP 학습 내용 정리, DFS 신규 문항 정리, 이론 학습 질문 내용 정리
[월]: 4시간 -이론 학습 질문 내용 정리
[화]: 3시간 - .. 일, 월 미비 사항 확인
[수]: 4시간 - .. 미비 사항 확인

매일 점심: DFS 문제 풀이
```

## 1. 스프링 필터와 인터셉터의 차이점을 설명하시오.
> a. #필터 와 #인터셉터 는 공통 관심사를 묶어 중복을 줄이기 위해 등장한 #AOP컨셉 의 구현체들 입니다.
- 같은 목적을 가졌지만 어디서 이런 처리를 수행하느냐의 차이가 있습니다.
- 두 구현체의 실행 시점의 차이에 대한 내용은 아래 기술합니다.

> b. 스프링 필터와 인터셉터의 실행 시점 차이
- 필터는 Web Container라고 Spring Context 밖에 서블릿 컨테이너 안에 들어가 있습니다.
<img width="764" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/f2a7ed78-ad24-4f86-99d2-2099ea5433f1">

```
따라서 필터는 Spring context 밖에 있어 주로 Bean을 사용하지 않는 전처리를 수행합니다.
예컨대 XSS, CORS정책 확인, jwt 토큰 확인, utf-8 등록 등 ..빈이 필요 없는 검증등을 수행합니다.
```

- 인터셉터는 필터를 지나 스프링 MVC의 디스패처 서블릿과 컨트롤러 사이에 존재합니다.
<img width="767" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/3cb9a9df-aa25-480d-a8ab-bfedfe1cbac4">
```
반면 인터셉터는 spring context 안에 들어있습니다.
그래서 dispatcher servlet을 넘어서 컨트롤러로 들어가기 전에 인터셉터를 타게 됩니다.
```


그래서 `사용 예시`를 보면 ..
- `필터`의 예시: UTF-8 인코딩
```
public class UTF8인코딩필터 implements Filter {

    @Override
    public void init(FilterConfig 필터구성) throws ServletException {
        // 필터 초기화 로직 (필요 시)
    }

    @Override
    public void doFilter(ServletRequest 요청, ServletResponse 응답, FilterChain 체인) throws IOException, ServletException {
        요청.setCharacterEncoding("UTF-8");
        응답.setCharacterEncoding("UTF-8");
        체인.doFilter(요청, 응답); // 다음 필터 또는 서블릿으로 요청 전달
    }

    @Override
    public void destroy() {
        // 필터 종료 로직 (필요 시)
    }
}
```

```
@Configuration
public class 필터구성 {

    @Bean
    public FilterRegistrationBean<UTF8인코딩필터> utf8인코딩필터등록() {
        FilterRegistrationBean<UTF8인코딩필터> 등록 = new FilterRegistrationBean<>();
        등록.setFilter(new UTF8인코딩필터());
        등록.addUrlPatterns("/*");
        return 등록;
    }
}
```


- `인터셉터`의 예시: 시간 측정
```
public class 요청시간측정인터셉터 implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest 요청, HttpServletResponse 응답, Object 핸들러) throws Exception {
        요청.setAttribute("시작시간", System.currentTimeMillis());
        return true; // true를 반환해야 요청이 컨트롤러로 전달됨
    }

    @Override
    public void postHandle(HttpServletRequest 요청, HttpServletResponse 응답, Object 핸들러, ModelAndView 모델앤뷰) throws Exception {
        long 시작시간 = (Long) 요청.getAttribute("시작시간");
        long 종료시간 = System.currentTimeMillis();
        long 처리시간 = 종료시간 - 시작시간;
        System.out.println("요청 처리 시간: " + 처리시간 + "ms");
    }

    @Override
    public void afterCompletion(HttpServletRequest 요청, HttpServletResponse 응답, Object 핸들러, Exception 예외) throws Exception {
        // 요청 완료 후 추가 작업 (필요 시)
    }
}
```

```
@Configuration
public class 인터셉터구성 implements WebMvcConfigurer {

    @Autowired
    private 요청시간측정인터셉터 요청시간측정인터셉터;

    @Override
    public void addInterceptors(InterceptorRegistry 레지스트리) {
        레지스트리.addInterceptor(요청시간측정인터셉터)
                  .addPathPatterns("/**"); // 모든 경로에 대해 인터셉터 적용
    }
}
```

> c. 필터에서 Reqeust를 래핑 하는 처리등도 진행하면 좋습니다.

```
public class HyeokSystemFilter implements Filter {
    
@Override
public void init(FilterConfig filterConfig) throws ServletException {}

@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, servletException {
    HyeokSystemRequestWrapper(HttpServletReqeust) request);
    .. 
	try { 
		RequestContext.initialize((HttpServletRequest) hyeokSysteRequest, (HttpServletResponse) response); 
	chain.doFilter(HyeokSystemRequest, response);
```

- 위 코드에서 왜 굳이 `HyeokSystemRequestWrapper`를 이용해 이용해 ServletRequest를 래핑해서 넘겨줄까?
- 저 ServletRequest의 경우는 스트림 형식이라 한번 읽으면 재사용이 안되서 그렇다.
- 그래서 저렇게 재사용 가능토록 처리해주는 역할을 Filter에서 적용해주면 좋다.


> d. 사실 필터에서도 빈을 쓸 수 있습니다.

- 버전업 되면서 #DelegationProxyFilter 가 나오면서 빈 등록 가능합니다.
- 따라서 필터를 인터셉터처럼 쓰기가 가능 하지만 되도록이면 기존에 필터의 목적에 맞게 구현하는게 관례입니다.


#이론학습


## 2. 스프링 @Transactional 애노테이션의 동작원리와 전파 속성들에 대해 설명하시오.
> a. 간략 답변
- 요약1
- 요약2
```
주석
```



## 3. LRU/LFU/FIFO 캐시 삭제 알고리즘에 대해 설명하시오.
> a. 간략 답변
- 요약1
- 요약2
```
주석
```



## 4. 소프트웨어 개발 중에 문제가 발생했을 때 어떻게 해결합니까?
> a. 간략 답변
- 요약1
- 요약2
```
주석
```


## 5. 이벤트 소싱과 CQRS 패턴에 대해 설명하시오
> a. 간략 답변
- 요약1
- 요약2
```
주석
```

