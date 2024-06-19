```
금주 한 것

1. ..

```


```
[금주 PLAN]
[일]: 9시간 - PJT 투자 (공통 모듈, 결제 도메인 개발, 결제 도메인에 공통 모듈 작성), DP 학습 내용 정리, DFS 신규 문항 정리, 이론 학습 질문 내용 정리
[월]: 4시간 - 이론 학습 질문 내용 정리 - 스프링 필터와 인터셉터의 차이 학습
[화]: 3시간 - 아무것도 못함
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


> a. 스프링 @Transactional은 애노테이션을 통해 말만(선언) 해두면 알아서 트랜잭션을 만들어줍니다.
- 저희가 JDBC 커넥션 객체를 생성하고 `autoCommit을 끈 뒤`에 `명시적으로 커밋토록` 하게 하고
- 롤백에 대한 처리를 해주면 ..
- 이게 하나의 트랜잭션이 됩니다.
```
public class TransactionExample {
    public static void main(String[] args) {
        Connection 연결 = null;
        PreparedStatement 조회명령문 = null;
        PreparedStatement 업데이트명령문 = null;
        ResultSet 결과셋 = null;

        try {
            연결 = DriverManager.getConnection("jdbc:yourDatabaseURL", "username", "password");
            연결.setAutoCommit(false);  // 자동 커밋을 끕니다.

            // 데이터 조회
            String 조회SQL = "SELECT 컬럼명 FROM 테이블명 WHERE 조건";
            조회명령문 = 연결.prepareStatement(조회SQL);
            결과셋 = 조회명령문.executeQuery();

            // 조회 결과에 따라 업데이트 수행
            if (결과셋.next()) {
                String 업데이트값 = 결과셋.getString("컬럼명");
                // 업데이트할 새 값 계산 로직 등...

                String 업데이트SQL = "UPDATE 테이블명 SET 컬럼명 = ? WHERE 조건";
                업데이트명령문 = 연결.prepareStatement(업데이트SQL);
                업데이트명령문.setString(1, 업데이트값);
                업데이트명령문.executeUpdate();
            }

            연결.commit();  // 명시적으로 커밋을 수행합니다.

        } catch (SQLException e) {
            try {
                if (연결 != null) 연결.rollback();  // 오류가 발생하면 롤백을 수행합니다.
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            e.printStackTrace();
        } finally {
            try {
                if (결과셋 != null) 결과셋.close();
                if (조회명령문 != null) 조회명령문.close();
                if (업데이트명령문 != null) 업데이트명령문.close();
                if (연결 != null) 연결.close();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }
    }
}

```


위 코드처럼 Connection 객체를 생성하고 commit을 하기 까지의 과정이 하나의 트랜잭션이 됩니다. 우리는 이 처럼 트랜잭션의 시작과 끝을 `트랜잭션의 경계`라고 표현합니다.


> b. 각 트랜잭션의 경계가 중첩되는 경우에 대한 처리 (propagation)
- 각 트랜잭션의 경계가 중첩되는 경우 어떻게 트랜잭션을 유지할 건지에 대한 옵션이 propagation이다. 
- 예컨대 아래와 같다.
	- required: 두 개의 `논리 트랜잭션`을 묶어 하나의 `물리 트랜잭션`으로 삼는다.
	- requiresNew: `새로운 물리 트랜잭션`을 만들어 `여러개의 물리 트랜잭션`을 운용토록 하는 것

> c. 논리 트랜잭션? 물리 트랜잭션 ..?
- 논리 트랜잭션이란 트랜잭션 매니저나 스프링에 의해 관리되는 논리적 단위의 트랜잭션을 말한다.
- 물리 트랜잭션은 DB Connection 하나당 하나씩 생성된다 (?)
	- 그럼 RequiresNew의 경우 2개 생성되는 건 DB 커넥션이 1개가 더 늘어난다는 것?

> d. 각 propagation 정책에 대해서 ..

(1) Required 계열 (수더분한 스타일)
- Required: 정~말 부모가 필요해서
	- * 부모가 있으면 참여
	- 없으면 새로 만들어

- Support: 적당히 지원 정도만 필요해서
	- * 부모가 있으면 참여
	- 없어도 그냥 진행

- Mandatory: 필요하다 못해 없으면 난리나서
	- * 부모가 있으면 참여
	- 없으면 예외 발생 (IllegalTransactionStateException)


(2) Requires_New 계열 (부모 있으면 삐딱선)
- Requires_New: 필요는 한데 .. 그냥 새 부모가 필요해
	- 부모가 있어도 새 트랜잭션 만들고
	- 없으면 당연히 새 트랜재션 만들고

-  Not_Supported: 부모 지원 필요 없고 혼자서도 잘해요
	- 부모가 있어도 새 트랜잭션 만들고
	- 없으면 그냥 진행

-  NEVER: 부모 절대 있으면 안돼!!!
	- 부모 있으면 에러
	- 없어도 안만들고 걍 고

-  NESTED: 부모고 뭐고 중첩해 걍
	- 부모 있으면 중첩
	- 없으면 생성

<img width="694" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/2681d190-353a-4f47-8da7-f49f478c0a33">



> e. 각 propagation 정책이 필요한 구체적인 상황들 ..

- (1) Required:
	- 주문 프로세스라는 부모 트랜잭션에서 상품 재고 감소, 결제등을 하나의 트랜잭션으로 합칠 때

- (2) Supported:
	- 부모 트랜잭션과의 합류가 선택적으로 이루어져야 하는 경우
	- 로깅과 같이 트랜잭션이 있을 경우 같이 작성되야 하지만 없어도 그런대로 진행되야 할 경우 적용

- (3) MANDATORY
	- 계좌 이체의 경우 반드시 기존 트랜잭션의 일부로서 수행되야 한다는 요구사항이 있는 경우
	- 계좌 이체만 누가 똑 빼서 실행치 않도록 ..

- (4) REQUIRES_NEW
	- 메시지 큐에 메시지를 보내는 경우 처럼 기존 트랜잭션과 상관 없이 독립적으로 실행돼야 하는 작업
	- 주문 처리중 오류가 발생하더라도 사용자에게 알림을 보내야 하는 트랜잭션은 반드시 실행되야함.

- (5) NEVER
	- 트랜잭션을 사용하면 안되는 것들을 다룰 때 사용
	- 주문 처리 중 여러 상품의 재고를 감소시키는 작업이 있을 때, 일부 상품 처리에서 실패해도 다른 상품 처리에 영향을 주지 않고 롤백할 수 있습니다. `NESTED`를 사용하면, 메인 트랜잭션 내에서 중첩 트랜잭션을 생성하여, 필요할 때만 해당 부분을 롤백할 수 있습니다.




## 3. LRU/LFU/FIFO 캐시 삭제 알고리즘에 대해 설명하시오.


> 1. 한정된 자원인 캐시가 넘치면 어떤 기준으로 삭제할건지에 대한 내용입니다.
- FIFO: 그냥 먼저 들어왔던 녀석 순으로 지우는 정책입니다. 구현이 쉽습니다.
	- 슈퍼마켓 계산대 처럼 먼저 들어오는 녀석을 먼저 삭제 해버립니다.

- LFU: 얘는 빈도수가 낮은 순으로 삭제하는 녀석입니다.
	- 카페의 단골 손님을 비정기적으로 오는 손님보다 우대하는 컨셉입니다.

- LRU: 얘는 가장 나중에 사용된 녀석부터 제거하는 녀석입니다.
	- 옷장 정리할 때 처럼 제일 자주 안 쓰는 녀석을 버려버립니다.

이런 캐시 삭제 정책은 히트율과 손실율이 어느정도 될지를 따져보고 결정해야 합니다.

> 2. 히트율과 손실율에 영향을 미치는 요소들

- 캐시 크기: 캐시 블락의 크기, 그냥 크게 잡을수록 맞출 범위가 넓어지는 거
	- 캐시 크기가 클 수록 많은 데이터를 저장할 수 있어서 히트율이 증가할 수 있습니다.

- 데이터 종류:
- 데이터 변동성:


> 3. 각 캐싱 정책이 쓰임직한 상황들 ..

- FIFO
	- `시간` 기반해서 시간에 따라 자주 사용하는 대상이 변하는 경우 좋습니다.
	* 실제로 자료 접근 빈도가 낮아 중요도가 크지 않은 환경등에 간단히 적용하기 좋을 거 같다.
	* 데이터 접근 빈도, 접근 량등이 일관적이지 않을 때

- LFU: 빈도수가 많은 것 (인기있는 항목 보여주는 ..)
	- `빈도수` 기반해서 자주 사용되는 걸 캐싱해야 할 때 쓰면 좋습니다.
	- 동영상 스트리밍 서비스에 인기있는 항목들 ..
	- 응용 프로그램 안에서 자주 사용되는 기능이나 모듈 등 ..
	- LFU를 사용하면 위 경우에 히트율을 높이고 손실율을 줄일 수 있다.

- LRU: 자주 접근하는 데이터가 많을 때
	- `시간` 과 `빈도` 기반해서 시간에 따라 자주 사용되는게 변화될 때 사용하면 좋습니다.
	- 실시간 인기 검색 관련 내용을 캐싱할 법 합니다.

> 4. 캐시의 종류

- L1, L2, L3 캐시
	- L1은 CPU 코어 내에 위치하는 캐시고 가장 빠릅니다.
	- L2는 코어당 하나씩 할당되거나 코어간 공유 될 수 있고 L1보단 느립니다.
	- L3는 여러 CPU 코어간 공유되는 캐시고 L1, L2보단 느리지만 메인 메모리보단 빠릅니다. 


> 5. 멀티 코어 환경에서의 캐시 일관성
- 캐시도 결국 저장소고 원본 저장소인 메모리로부터 값을 복사해오는 거기에 멀티 코어 환경에서 동시성 이슈가 발생할 수 있다
- 이를 해결하기 위해 3가지 기본적인 프로토콜이 존재한다.
	- MSI: 
		- Modified: 특정 캐시에서 데이터 변경시 다른 캐시에는 이 데이터가 동기화되지 않는데 이 때 유일하게 변경된 캐시 라인을 Modified 상태로 표현
		- Shared: 해당 데이터가 모든 캐시에 동기화 된 상태를 의미. 현재 데이터는 어딘가에서 수정되지 않은 완전한 가시성을 보장함을 표현
		- Invalid: 다른 캐시로부터 데이터가 변경되어 특정 데이터가 Modified가 되었을 때 데이터가 동기화 되기 전 상태를 표현
	- MESI
		- Exclusive 추가: 이 상태의 캐시 라인은 한 코어에만 존재하는데 수정되기 전 상태이다. 그냥 수정 없이 읽기만 할 때 사용. 프로세서가 독점적인 데이터를 가지고 있을 때 불필요한 무효화 방지를 통해 성능 향상
	- MOESI
		- Owner 추가: 이 상태의 캐시 라인은 수정된 데이터의 최신 버전을 가지고 있지만 다른 캐시에도 해당 데이터가 존재할 수 있음.  .. 사실 MESI랑 MOESI의 컨셉은 잘 이해 못했음.


* 캐시라인: 캐시 메모리에서 데이터를 관리하는 기본 단위. 


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

