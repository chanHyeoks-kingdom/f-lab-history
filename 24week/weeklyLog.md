 # @23 주차 멘토링 회고


## Topic 1.저희가 예전에 예외처리 했던 것 기억 하시나요? 
> Q1. 예외를 본인이 직접 처리 안하고 호출한 곳에게 처리를 위임하는 건 어떤 장점이 있을까요?
- 중앙 집중화된 처리를 가능케 한다.
```
아예 똑같은 예외 처리가 필요하다고 하면 중복 코드를 줄일 수 있습니다.
```

<br>

> Q2. 아래 경우에  대해선 어떻게 생각하세요?
- `validateUser`라는 메서드에서 예컨대 `나이가 어리다`라거나 `성별이 잘못되었다`라는 서비스 정책에 맞지 않는 상황에 대해 `exception을 발생`시켜야 할 때 
- `invalidAge`와 `invalidGender`를 나누는 방법과 그냥 `invalidException`라고 하나로 통합해서 나누는 방법이 있을텐데 저희가 예전에 예외처리 했던 것 기억 하시나요?


<br>

> Tip 1. 중앙 집중화된 처리와 개별적인 처리중 어떤 것을 선택해야 할지에 대한 `의사 결정 기준`
- 결국 `처리의 방법`에 따라 컨트롤러 어드바이스와 같이 `중앙 집중화된 처리`를 사용하거나 `예외 발생 지점에서 목적에 맞는 에러`에 대한 의사결정을 하면 좋다.

```
- 여러 군데서 30개 에러에 대한 처리가 똑같이 처리해야 한다면 이건 좋다. 말이 된다.
- 근데 컨트롤러 어드바이서에 한 군데에 30개의 익셉션 처리를 다 따로 잡아서 처리하면 이건 중앙 집중화를 할 이유가 없다.
```

<br>

> Tip 2. 문제가 발생한 지점. 그곳이 가장 컨텍스트가 많아요.
```
- 예를 들어서 제가 결제를 요청했는데 결제가 실패했을 경우
- 그 전에 있던 것들을 롤백 할 것인가, 사가 패턴 적용할 것인가는 그 지점에서 알 수 있다.
- 만약 컨트롤러 어드바이서로 끝까지 올릴 경우 사실 거기서 해줄 수 있는 건 많이 없다. (재시도 처리 등 ..)
```



<br>

---


## Topic 2. Throws를 안하고 처리한 결과만 String으로 내려주는 방법?
> Q1. 예외 메시지와 응답 메시지의 차이는 뭔가요?
- 말씀하신 방법은 그러니까 .. `예외 메시지라기 보단 응답 메시지인거네요?` 응답 메시지에 에러 내용을 담은?

<br>

> Q2. 예외와 응답의 차이는 뭐예요?
- 예외라는 건 `예상하지 못한 에러`예요. 발생하면 안되는건데 발생 되었다면 그건 예외예요
```
case1. 그래서 결제가 실패한다는 건 예외에요. 사용자가 카드를 긁었을 때 이건 무조건 결제가 되어야 하는건 기본이잖아요?
> 거기서 예외가 발생한다면 그건 예외예요.
```
```
case2. 회원가입 과정에서 데이터베이스에 현재 사용자를 조회했을 때 이미 같은 ID가 있는 경우.
> 이건 충분히 비즈니스 상에서 예상 가능한 상황이죠? 그럼 이건 `예외가 아닌거죠`. 즉, 발생할 수 있는 상황이니까 `응답에 넣어 보내줄 수 있어요.`
```

따라서 `예외로서 내린다`라던가 `응답 메시지에 담아 내린다` 라는데 의사 결정은 이게 `노멀 케이스상 예상할 수 있는 문제인가`를 전제하면 좋다.

<br>

> Q3. Map을 사용한 분기도 충분히 좋은 예시입니다.
- 실제로 유연한 구조를 제공해야 한다는 요구사항이 있는 경우에 쓰면 좋습니다.


## 미비사항

> Q1. DNS에는 몇 단계가 있나요?
-
-
```

``` 


> Q2. IP를 주소로 바꿀 때 어디서 캐시가 되는지 혹시 ..?
-
-
```

```


> Q3. 주소 탐색 과정이나
-
-
```

```

> Q4. TCP/IP는 어떻게 동작하는지 아시나요?
-
-
```

```


> Q5. 호주에서 한국에 있는 웹 사이트에 접속을 하면 그 이미지를 어디서 가져올까요?
-
-
```

```


> Q6. CDN의 문제점은 뭐가 있을까요?
- CDN 내부에서 문제가 생기면 모든 파일을 못가져오는 문제가 생길 수 있다. 가장 큰 CDN 라우티지가 발생했을 때 전세계의 50% 웹이 마비되는 문제가 있었어요. (검색해보기)
- 설치 비용
```
...
```


> Q7. 예를들어 테이블 7개가 있을 때 7개를 다 조인해야 할 때 View가 줄 수 있는 장점은?
- Join 과정을 사용자가 굳이 JOIN을 쓰지 않아도 된다.

> Q8. 데이터베이스 뷰는 데이터를 저장 하나요?
- 실제 저장은 하지 않고 SELECT문이 내부적으로 동작합니다.

> Q9. volitile는 저장되어 있는 값을 어디서 가져오나요?
- 메인 메모리입니다!
```
캐시는 사실 복사본이어서 멀티 스레드 환경에서 의미가 없다.
```

> Q10. volitile을 썼을 때 왜 문제가 생기는지 조사해 보면 좋다. (원자성이 아닌 가시성 문제)
- 
```

```

<br>

> Q11. RDBMS와 NoSQL
- 몽고 DB는 Json 형태로 저장된다. Json 구조의 경우 테이블과 다르게 스키마적으로 확장성이 있다.
- 트리 형태로 계속해서 유연하게 데이터 구조를 늘릴 수 있다.
```
JSON과 바로 매핑할 수 있도록 하는 건 Map 구조가 더 적합해 보이는데 ..?
```

<br>

> Q12. CAP는 왜 필요했죠?
- 답변: 분산 환경 때문입니다!
- 추가 질문: 분산 환경은 왜 필요했죠?
```
- * (중요) 대용량 데이터 처리에 도움이 되기 때문에
- 데이터를 안전하게 저장하고
- 가용성을 늘려주고
- 빠르게 데이터를 전달하기 위해서
```
그래서 `로깅이나 사용자 실시간 데이터`는 NoSQL이 잘 어울리고 `트랜잭션이 중요한 경우`는 RDBMS가 좋다.

<br>

> Q13. 자바 쓰레드의 생명 주기는 왜 알아야 할까요?
- 한 애플리케이션에서 쓸 수 있는 쓰레드의 개수는 정해져 있고
- 대기 상태에 많은 스레드가 계속 몰려 있으면 좋지 않습니다.
- 유휴 시간이 많으면 좋을까요? 짧아야 합니다. 만약 이런 대기 시간이 많으면 문제가 있는 시스템이겠죠?
- 이런 내용을 이해해야 하기 때문에 생명 주기를 알야아 합니다.
```
* 이런 내용은
- 문제 재시작 하기 전에 쓰레드 덤프 써줘.
- 메모리 캡처시
```

