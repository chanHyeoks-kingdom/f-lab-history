

# WEEK 6

```
[이 주의 과제]
1. JDBC에 대해 설명하시오 (키워드: 드라이버, 트랜잭션, Statement vs PreparedStatement, ResultSet)
2. DB Connection Pool의 동작원리에 대해 설명하시오
3. 데이터베이스 clustered index vs non-clustered index에 대해 설명하시오
4. 객체 지향 설계 5원칙 - SOLID (vs 다른 개발 원칙)
```



## [자문자답]


----------


#### 1. JDBC에 대해 설명하시오 (키워드: 드라이버, 트랜잭션, Statement vs PreparedStatement, ResultSet)
> JDBC는 자바 코드로 데이터베이스를 다루기 위해 필요한 인터페이스 입니다. 각 DBMS는 이를 구현한 JDBC 구현체를 제공하는데 이게 바로 드라이버 라는
> 것 입니다. 이 드라이버로 부터 받을 수 있는 커넥션을 통해 우리는 preStatement나 Statement를 정의하고 이를 이용해 커밋할 수 있는데 preSatatement는
> '?'라는 플레이스 홀더에 대해 동적으로 setString()하는 방식을 사용하고 있어서 하나의 메서드에서 특정 조건 값만 변경하는 처리 로직이 많이 반복될 때
> 이점이 있습니다. 또 setString()을 하는 과정에서 sql injection을 방지하게 되서 보안적인 측면에도 이점이 있습니다. 반면 Statement는 정적 쿼리를 직관
> 적으로 쓸 수 있다는 이점이 있어서 이런 측면들을 고려해 쿼리를 만든 뒤 DB의 쿼리를 보내기 위해 execute()를 해야합니다. 이 떄 default 설정은 이 작업과 > 함께 커밋이 일어나는데 복합적인 쿼리에 대해서 트랜잭션을 보장할 필요가 있다면 Connection 객체의 setAutocommit(false)메서드를 호출해 자동 커밋을 끈
> 다음 직접 commit(), rollback() 메서드를 이용해 트랜잭션 처리를 해주어야 합니다. 이런 작업 중 SELECT 관련 작업의 결과는 executeQuery() 메서드를 이용
> 해 ResultSet이라는 형태로 받을 수 있는데 이 클래스의 getString()메서드를 이용해 필드 값등을 추출하는 작업등을 할 수 있습니다.

###### TODO 1. JDBC 사용할 때 고려할 수 있는 DAO 패턴이나 
###### TODO @. JDBC 활용 best practice 작성해보기

<br>

> ##### 꼬리질문1. DB_CONNECTION 등의 작업을 수행할 때 내부적으로 JDBC api를 구현한 구현체(JDBC Driver)가 필요하다는 말씀이신가요

###### 네! 예를 들어 아래와 같은 작업을 수행시에 때 java.sql의 Driver라는 인터페이스를 구현한 구현체가 필요합니다! 

```
Connection connection = DriverManager.getConnection(url, user, password);
```
###### 위 인터페이스에 대한 mysql의 JDBC driver의 구현체는 아래와 같고 Connection을 얻는 동작을 수행하게 됩니다.

```
# getConnection() in DriverManage.class

        for (DriverInfo aDriver : registeredDrivers) {
            // If the caller does not have permission to load the driver then
            // skip it.
            if (isDriverAllowed(aDriver.driver, callerCL)) {
                try {
                    println("    trying " + aDriver.driver.getClass().getName());
                    Connection con = aDriver.driver.connect(url, info);   // TODO 1. 
                    if (con != null) {
                        // Success!
                        println("getConnection returning " + aDriver.driver.getClass().getName());
                        return (`con`);
                    }
                } catch (SQLException ex) {
                    if (reason == null) {
                        reason = ex;
                    }
                }

            } else {
                println("    skipping: " + aDriver.driver.getClass().getName());
            }

        }
```

<br>


###### 결국 저 registeredDrivers 라는 건 DriverInfo 리스트인데, registerDriver라는 걸 통해 레지스터를 등록할 때 사용됩니다.
```
# registerDriver() in DriverManager class

private static final CopyOnWriteArrayList<DriverInfo> registeredDrivers = new CopyOnWriteArrayList<>();

..
    public static void registerDriver(java.sql.Driver driver)
        throws SQLException {

        registerDriver(driver, null);
    }
..
```

<br>

###### 그럼 저 registerDriver는 언제 호출되서 registeredDrivers에 각 JDBC Driver가 들어가는걸까요?
```
package com.mysql.cj.jdbc;

import java.sql.SQLException;

.. 
public class Driver extends NonRegisteringDriver implements java.sql.Driver {

    // Register ourselves with the DriverManager.
    static {
        try {
            java.sql.DriverManager.registerDriver(new Driver());
        } catch (SQLException E) {
            throw new RuntimeException("Can't register driver!");
        }
    }

    /**
     * Construct a new driver and register it with DriverManager
     *
     * @throws SQLException
     *             if a database error occurs.
     */
    public Driver() throws SQLException {
        // Required for Class.forName().newInstance().
    }

}
```

###### 위 코드는 mysql의 Connector/J (JDBC Driver in mysql)에서 제공하는 Driver 클래스의 내용입니다! 결국 sql의 Driver를 구현한 구현체인데, 클래스 자체에서 DriverManager의 registerDriver()를 호출해 자신을 등록해줍니다.


> ###### public static Connection getConnection(String url, ..) # getConnection은 static이어서 `DriverManager.getConnection(url, user, password);` 처럼 사용할 수 있음

<br>
<br>



----
> ##### 꼬리질문2. PreparedStatement 말고 Statement도 있었던 거로 기억하는데 어떤 기준으로 선택하면 좋을까요?
```
PreparedStatement는 미리 SQL을 컴파일 해서 '?' 같은 플레이스 홀더로 지정해둔 파라미터만 setString(), setInt()등의 메서드를 사용해 동적으로 처리할 수 있기 때문에 실행 횟수가 많아질수록 성능상의 이점을 가져갈 수 있습니다. 또 미리
SQL을 처리한다는 특성 덕분에 일부 SQL 인젝션을 방지할 수 있습니다. 
```
###### TODO 1. setString()이 왜 sql 인젝션을 방지하는가 .. 기존의 String에 + 연산을 이용한 방법이 sql injection에 취햑함은 확인



<br>
<br>



----
> ##### 꼬리질문3. JDBC를 쓸 때 트랜잭션 얘기 해주셨는데 그게 뭔가요?
```
트랜잭션은 간단히 말하면 '작업의 단위'인데요, 예를 들어 계좌 이체 업무의 경우 '(1) 잔고 조회, (2) 금액 비교, (3) 송금 요청, (4) 송금 가능 확인, (5) 현재 잔고 줄이고 상대 잔고 올리기' 같이 어떤 작업의 최소 단위를 트랜잭션이라고 합니다.
근데 jdbc의 경우 쿼리를 실행하는 순간 commit을 하는 autoCommit 설정이 default여서 connection.setAutoCommit(false); 등의 작업을 통해 commit을 수동으로 변경 후 작업 완료시에 commit토록, 그 사이에 문제 발생시 conn.rollback()등의 작업을
수행해야 합니다.
```

----
> ##### 꼬리질문4. 트랜잭션이 뭔가요?
```
이건 할 말 겁나 많아서 일단 보류 ..
```

@ preparedStatement, ResultSet, DB Connection을 닫아주지 않는 경우에 생기는 문제도 고민해보기.
@ 여기서 @Transactional과 함께 PSA 개념도 설명 가능
@ 또 Connection의 경우에는 static 객체 객체이기 때문에 사용이 끝났으면 꼭 close()를 해줘야 합니다. <- 이 내용 자세히 보기


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


----------
----------

##### 2. DB Connection Pool의 동작원리에 대해 설명하시오 [[참고1], ](https://dkswnkk.tistory.com/685)
```
앞서 봤듯이 커넥션은 객체다. 이런 객체를 계속 생성하는 것도 비용이 드는데, 사실 더 중요한 건 DataBase에 연결하는 것이 TCP/IP 통신이라는 부분이다. 때문에 연결, 해제를 할 때에 시간이 많이 소요된다. 사실 통신이란게 비용을 많이 먹는다.
그래서 애플리케이션 뜰 때 객체들을 미리 생성해두고 그걸 할당해주는 방식으로 하는게 DB Connection Pool이고, 이는 java.sql에 보면 DataSource라는 인터페이스가 있는데 getConnection등의 커넥션 획득 방법들을 관리하는 인터페이스다.
이걸 구현할 때 쓰레드 풀등을 제공 및 적용할 수 있는데 이런 방법을 사용하는 것에는 예컨대 HikariCP 같은 경량화된 DB Connection Pool등이 있다.
```
> ###### `중요한 건, HikariCP를 사용하려면 별도의 JDBC Driver가 존재해야 한다는 것. 서로 상호작용 하는 것  뿐`


<br>

----
> ###### 꼬리질문 1. Connection Pool은 얼마가 돼야 적당할까요?
```
사실 무조건 커넥션 풀을 늘린다고 성능이 늘어나진 않습니다. 어차피 해당 커넥션을 다룰 쓰레드도 필요한데 이건 별도의 쓰레드풀에 의해 관리될 가능성이 있어서
이런 상관관계도 고려해야합니다. '그럼 쓰레드 풀도 늘리면 되는 거 아냐?'라고 할 수 있지만 결국 쓰레드나 커넥션도 객체이기 때문에 메모리에 악영향을 줄 수 있습니다. 또 쓰레드가 동시 작업이란 것도 결국 CPU에 대해 문맥 교환을 일으키면서 번갈아 받는거여서 이로인한 성능상의 한계가 존재합니다. 커넥션도 마찬가지구요. 사실 DB 데이터를 다루는 디스크 접근 자체도 성능상의 한계가 존재합니다.
```


----
> ###### 꼬리질문 2. 그래서 뭐 어쩌라는 건가요? (chatGPT 한테도 물어보기)
```
mysql 공식 문서의 경우 600명의 유저에 대해 15~20개의 커넥션이, 히카리 CP 공식문서의 경우 `pool size = tn * (cm - 1) + 1`이라는 공식을 언급하긴 하는데
아직 잘 모르것습니다. (장형철 튜터님 기준 보기)
```




##### 3. 데이터베이스 clustered index vs non-clustered index에 대해 설명하시오
```
클러스터 인덱스는 마치 전화번호부처럼 데이터 자체를 저장하는 순간부터 물리적으로 정렬해둬서 찾기 편하게 합니다. 근데 이 저장에도 비용이 듭니다.
반면 논 클러스터 인덱스는 검색 조건별로 여러개의 별도 테이블을 둬서 인덱싱 하는 방법인데, 아무래도 별도 테이블에 대해 조인하다 보니 속도가 느립니다.

@ 실제 구현 방법 확인해보기
@ 데이터가 언제 정렬되는지 검토해보기
@ 인덱스를 구분하는 다른 방법들 확인해보기
@ 인덱스 조회할 때, 인덱스 생성할 때에 대해서 살펴보기
```
> ..
> ..
> ..


##### 4. 객체 지향 설계 5원칙 - SOLID (vs 다른 개발 원칙)
```
흔히 S(Single Responsibility Principle), O(Open/Closed Principle), L(Liscov Substitution Principle), I(Interface segregation principle), D(Dependency inversion principle)은
객체지향을 설명하라고 할 때 가장 자주 나오는 개념중에 하나인데요, 사실 별 거 없고 각 원칙들을 지키라는 얘기입니다. 예컨대 단일 책임 원칙은 클래스는 '하나의 책임'만 가지고 있으라는 말이구요, 개방폐쇄 원칙 같은 건 의존성이 있는
객체의 내용이 변경되도 내부 요인이 변경되지 않도록 하라는 의미입니다. 인터페이스 분리 원칙은 그냥 구현체 직접 쓰지 말란 얘기고, 의존관계 역전은 의존성 주입 받아 쓰라는 얘기입니다.
```
> ..
> ..
> ..













<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

```
[그냥 부록: 개소리 모음]
# getConnection() in mysqlDataSource.class

protected java.sql.Connection getConnection(Properties props) throws SQLException { // 이거는 커넥션이랑 상관 없음, 그냥 DataSource 인터페이스 구현체라도 커넥션 풀 다루는거랑 관계 있는게 아니라 그냥 실제 커넥션 넣을때 필요한 ID, PORT등 설정하는거임
    String jdbcUrlToUse = this.explicitUrl ? this.url : getUrl();

    //
    // URL should take precedence over properties
    //
    ConnectionUrl connUrl = ConnectionUrl.getConnectionUrlInstance(jdbcUrlToUse, null);
    Properties urlProps = connUrl.getConnectionArgumentsAsProperties();
    urlProps.remove(PropertyKey.HOST.getKeyName());
    urlProps.remove(PropertyKey.PORT.getKeyName());
    urlProps.remove(PropertyKey.DBNAME.getKeyName());
    urlProps.stringPropertyNames().stream().forEach(k -> props.setProperty(k, urlProps.getProperty(k)));

    return mysqlDriver.connect(jdbcUrlToUse, props);
}

```
> ##### 위 코드에서 얻어내는 `connect()의 리턴 값`이 `DriverManager.getConnection(..)`로 부터 받는 connection 객체인데, 우리가 `PreparedStatement preparedStatement = connection.prepareStatement(query);`할 때 받는 객체는 아래와 같다.
```
// in com.mysql.cj.jdbc.ConnectionImpl.class (1523 line)
@Override
public java.sql.PreparedStatement prepareStatement(String sql) throws SQLException {
    return prepareStatement(sql, DEFAULT_RESULT_SET_TYPE, DEFAULT_RESULT_SET_CONCURRENCY);
}
```

> ##### 일단 내가 말하고 싶은 건 결국 저 connection 객체로부터 prepareStatement 라는 녀석이 생성을 받아올 때 ClientPreparedStatement라는 걸 준다는 얘기야 `PreparedStatement`를 상속받는 녀석이라 직접적으론 보이지 않지만 어쨌든,
```
    @Override
    public java.sql.PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency) throws SQLException {
        synchronized (getConnectionMutex()) {
            ClientPreparedStatement pStmt = null;
            ..

            return pStmt;
        }
    }
```

> ##### 결국 저희가 preparedStatement.setString(..)같은 걸 사용 할 때는 아래 코드를 사용하는건데, '?' 플레이스 홀더에 들어갈 부분을 setString()으로 입력하는 걸 볼 수 있습니다. 저 경우 SQL 명령어로 해석되지 않아 문제가 발생치 않게 됩니다.
```
    @Override
    public void setString(int parameterIndex, String x) throws SQLException {
        synchronized (checkClosed().getConnectionMutex()) {
            ((PreparedQuery) this.query).getQueryBindings().setString(getCoreParameterIndex(parameterIndex), x);
        }
    }
```
