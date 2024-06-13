```
[금주 PLAN]
[월]: 4시간 -PJT 투자 (서비스에서 DTO 변환토록, 통합 테스트 개선, 공통 모듈 작성, 도커 환경 구성)
[화]: 기술 질문 다 정리 (3시간)
[수]: 결제 도메인 간단히 구현 (4시간)

매일 점심: DP 학습
```

## 1. Java Thread 의 생명 주기에 대해 설명하시오

> a. 6가지 상태
- Java Thread는 운영체제의 커널 쓰레드의 상태를 연동하기 위해 존재하는 구분이다.
```
Runnable, Running, Terminate가 있고
대기 상태 전환을 위한 Wait, Time Wait 상태가 있다.
이미 Lock을 선점당한 자원에 접근시 전환되는 Blocked 상태도 존재한다.
```

> b. 각 상태로 전환되기 위해 전제되어야 하는 것들
- 맨 처음 Tread 객체를 Runnable 상태로 만들기 위한 start() 메서드들을 활용할 수 있다.
- Waiting 상태로 가기 위한 sleep, join, wait등의 키워드들이 있다.
- 특정 스레드의 대기상태를 강제로 해제하기 위한 intruptted()등의 액션 등이 있다.
- 자원을 빠르게 해제해 Blocked 상태의 스레드를 빨리 Runnable로 바뀌기 위한 free 액션등이 있다.

> c. 더 찾아봐 ..
- 똥구멍
- ..

<br>

## 2. 웹브라우저에 url 입력 후 일어나는 일들에 대해 설명하시오.

> a. 웹 브라우저를 이용한 HTTP 통신의 4단계
- 크게 url 파싱, request 생성, response 수신, 랜더링의 4 단계로 구분된다.
```
- 먼저 url을 파싱해 입력받은 도메인 명에 대한 IP 주소가 IP 테이블이나 브라우저 캐시에 존재하는지 확인한다.
- 없을 시 DNS를 이용해 해당 도메인의 IP 주소를 찾아온다.
- 요청 메시지를 HTTP 리퀘스트로 만들어 전송한다.
- HTTP 응답을 받는다.
- 화면에서 해당 페이지를 랜더링한다.
```

<br>

## 3. 데이터베이스 뷰(view)와 장단점을 설명하시오

> a. 뷰의 컨셉
- 데이터 베이스 뷰는 SELECT 문을 함수처럼 쓸 수 있도록 지원하고 권한을 통해 해당 결과 조회 대상을 통제하는 컨셉이다.
```
사실 별도 공간을 둬서 조회용 테이블을 만드는게 아니다보니 매번 해당 SELECT 문을 실행시켜 주어야한다.
```

> b. 뷰의 성능
- VIEW는 쿼리시 마다 조회 작업이 일어나기에 JOIN등에 의해 부하가 걸릴 수 있다.
```
- 자주 사용되는 뷰에 대해 인덱스 생성
- 물리적인 인덱스를 가진 물질화된 VIEW 생성
```

> c. VIEW 사용시 주의점
- VIEW는 내부적으로 SELECT하는 대상이 되는 테이블에 실시간으로 의존하고 있다.
```
VIEW는 결국 SELECT 해주는 함수 정도의 개념이기 때문에 별도 테이블이 생성되는게 아니라 매번 해당 SELECT
문을 실행시켜주는 것이어서 FROM의 대상이 되는 테이블이 변경되는 경우 이에 맞춰 VIEW 코드를 변경해줘야 한다.
```

> d. VIEW EXAMPLE
- 뷰를 생성하는 키워드는 CREATE VIEW를 사용하고 뷰 이용시엔 그냥 테이블 조회처럼 사용하면 된다.
```
CREATE VIEW CustomerOrders AS
SELECT c.CustomerID, c.Name, o.OrderID, o.OrderDate, o.Amount
FROM customers c
JOIN orders o ON c.CustomerID = o.CustomerID;
```
```
SELECT Name, OrderID, OrderDate, Amount
FROM CustomerOrders
WHERE CustomerID = '123';
```


> e. VIEW에서 권한을 다루는 방법
- VIEW에서 특정 사용자에게 권한을 주고 뺐는 방법은 GRANT, REVOKE 키워드를 사용한다.
```
-- 'username' 사용자에게 'EmployeeView' 뷰의 SELECT 권한을 부여
GRANT SELECT ON EmployeeView TO username;
```
```
-- 'username' 사용자의 'EmployeeView' 뷰에 대한 SELECT 권한을 제거
REVOKE SELECT ON EmployeeView FROM username;
```
```
-- 'username' 사용자에게 'EmployeeView' 뷰의 SELECT, INSERT, UPDATE 권한을 부여
GRANT SELECT, INSERT, UPDATE ON EmployeeView TO username;
```

<br>

## 4. Java의 Atomic, Volatile, Synchronized 각각의 차이에 대해 설명하시오
- Synchronized는 메소드나 블록을 동기화하여 한 시점에 하나의 스레드만 접근할 수 있도록 합니다.


> a. synchronized 사용패턴 (1): 메소드 동기화 
```
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

> b. synchronized 사용패턴 (2): 블록 동기화 
```
public class Counter {
    private Map<String, String> cache = new HashMap<>();

    public void updateCache(String key, String value) {
        synchronized (cache) {
            cache.put(key, value);
        }
    }

    public String getValue(String key) {
        synchronized (cache) {
            return cache.get(key);
        }
    }
}
```
- cache 같은 객체에 syncrhonized를 걸어두고 해당 블록 안에 동시성을 유지시키고 싶은 로직을 두는 방식이다.
- 만약 cache 자체의 동시성 문제를 핸들링 하고 싶다면 이와 관련된 모든 작업에 SYNCRHONIZED를 걸어줘야 한다는 한계가 있다.

> c. synchronized 사용패턴 (3): 클래스 레벨 동기화 
```
public class Service {
    private static int count = 0;

    public static synchronized void increment() {
        count++;
    }
}
```
- 모든 Service 객체들에서 increment를 접근할 때 동기화를 거치게된다.
- 마찬가지로 count는 synchronized와 관계 없는 곳에선 열려있는 상태여서 동시성 문제를 조심해야한다.

> d.synchronized 사용패턴 (4): 정밀한 객체 동기화
```
public class CustomLock {
    private final Object lock = new Object();
    private int data = 0;

    public void increment() {
        synchronized (lock) {
            data++;
        }
    }
}
```



> b. volatile
- Volatile 키워드는 변수를 메인 메모리에 저장하게 하여 캐시가 아닌 메인 메모리에서 직접 읽고 쓰게 합니다. 이는 모든 스레드에 변수의 최신 값을 보장합니다.
```
public class Flag {
    private volatile boolean flag = true;

    public void toggle() {
        flag = !flag;
    }

    public boolean getFlag() {
        return flag;
    }
}

장점:
간단한 상황에서 가볍고 빠른 동기화를 제공합니다.
변수의 가시성을 보장하며 쉬운 사용법을 가집니다.

단점:
복잡한 상태나 작업을 동기화하기에는 부적합합니다.
volatile은 원자성을 보장하지 않습니다.
```

> c. Atomic
- Atomic 클래스는 java.util.concurrent.atomic 패키지에 속해 있으며 lock-free 프로그래밍을 가능하게 합니다.
- 단일 변수에 대한 원자적 연산을 지원하여 멀티 스레딩에서의 데이터 무결성을 보장합니다.
```
import java.util.concurrent.atomic.AtomicInteger;

public class SafeCounter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}


장점:
성능 향상을 제공합니다. Lock을 사용하지 않으므로 오버헤드가 적습니다.
코드가 간결하며 사용하기 쉽습니다.

단점:
복잡한 연산을 수행할 때는 적합하지 않을 수 있습니다.
주로 단일 변수에 대한 간단한 연산에 적합합니다.
```

<br>

## 5. RDBMS와 NoSQL의 차이에 대해 설명하시오.

> a. 대략적인 차이
- (구조) RDBMS는 `테이블이라는 고정된 형식`을 갖고 NoSQL은 `키-값`, `문서`, `그래프`등의 다양한 형태를 지원한다.
- (확장성) RDBMS는 `수직 확장`, NoSQL은 `수평 확장`
- (트랜잭션) RDBMS는 `ACID`, NoSQL은 `CAP`
- (쿼리 성능 측면) RDBMS는 SQL를 이용해 복잡한 쿼리나 조인 수행에 적합, NoSQL은 단순 조회, 쓰기에 최적화
  
* 결론:
  
    - RDBMS는 `복잡한 트랜잭션에 데이터 무결성이 필요한 은행등`에 적합하고,
    - NoSQL은 `빠른 확장성과 유연한 데이터 처리가 필요한 소셜미디어`나 실시간 데이터 분석 플랫폼에 적합하다.
```

```

<br>

## 6. 이벤트 소싱과 CQRS 패턴에 대해 설명하시오

> a. 요약
-
```

```

<br>

## 7. DP에 대해서

> a. 요약
-
```

```



<br>

> # PJT!!
```
1. 서비스에서 DTO 변환토록
2. 통합 테스트 개선
3. 공통 모듈 작성
4. 도커 환경 구성
5. 결제 도메인 간단히 구현
6. ...
```


