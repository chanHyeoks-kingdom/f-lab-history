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
> a. 똑같이 AOP라는 컨셉의 구현체고 실행 시점의 차이가 있습니다.
- 필터는 Web Container라고 Spring Context 밖에 서블릿 컨테이너 안에 들어가 있습니다.
- 인터셉터는 필터를 지나 스프링 MVC의 디스패처 서블릿과 컨트롤러 사이에 존재합니다.
<img width="764" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/f2a7ed78-ad24-4f86-99d2-2099ea5433f1">
<img width="767" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/3cb9a9df-aa25-480d-a8ab-bfedfe1cbac4">

```
보다싶이 필터는 Spring context 밖에 있어서 Bean을 못씁니다.
그래서 XSS, CORS정책 확인, jwt 토큰 확인, utf-8 등록 등 ..빈이 필요 없는 검증등을 수행합니다.
```

```
# 정리할 내용 아래

A5. Interceptor

인터셉터는 뭐하는 놈일까요?



필터 다 거르고 spring context안에 들어왔죠?

그럼 http request가 컨트롤러로 보내지고, 서비스로 보내지고 등등 해서 처리되겠죠?

이 때, 컨트롤러에서 회원가입 메서드에서 파라미터로 UserVO를 받는데, 필드값이 username, password인데,

client에서 POST요청 보낼 때

{
	usermame: xxx,
	pazzword: xxx
}

이런식으로 필드 이름을 오타내서 보냈다고 하면, 이걸 컨트롤러 단에서 파라미터로 받을 때

@Valid UserVO 로 확인한 후, 필드 값이 이상하면 Exception을 던지는데요,

여기서 @Valid나, 컨트롤러 안에서 터지는 Exception을 잡아서 Exception만 전용으로 던지는 @ControllerAdvice 이게 붙은 핸들러들을 인터셉터라고 보시면 됩니다.



이 외에도 서비스 단에서 비지니스 로직 처리에 걸리는 시간 앞 뒤로 해서 측정하는거나,

스프링에서 로깅 찍을 때,

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

private Logger logger = LogManager.getLogger(log.class);
logger.info("로깅!");

이러는 애들도 일종의 인터셉터라고 보시면 됩니다.





A6. 필터에서 빈을 못쓴다?



근데 사실 필터에서는 스프링 빈 못쓴다는거 뻥입니다ㅋㅋ

원랜 못썼는데요,

버전업 되면서 DelegationProxyFilter가 나오면서 빈 등록 가능합니다.

그래서 필터를 인터셉터처럼 쓰기가 가능은 한데요,

되도록이면 기존에 필터의 목적에 맞게 구현하는게 관례입니다.

(spring context안에서 놀다가 web context 밖에 나왔다가 다시 들어가는게 아마 성능상으로도 별로일 거라고 추측하고 있습니다..)

```



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

