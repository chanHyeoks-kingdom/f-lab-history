

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
```
JDBC는 자바 애플리케이션에서 데이터베이스에 접근하기 위해 필요한 표준 API 입니다. 자바에서 제공하는 java.sql을 통해 DB CONNECTION을 받고 쿼리를 요청하는 일들을 할 수 있는데 이 때 필요한 JDBC API를
실제 각 벤더별로 구현한게 JDBC Dirver입니다. 이 과정에서 쿼리 생성과 실행은 PreparedStatement등을 사용하고 응답 등은 ResultSet을 통해 DB에서 얻어온 데이터를 사용할 수 있습니다. 근데 이 때 기본적으로
쿼리를 execute 할 때 자동으로 커밋토록 되어 있는데 이러면 트랜잭션을 관리할 수 없기 때문에 필요시에는 Connection 객체의 setAutoCommit(false) 설정이나 commit(), rollback() 메소드를 활용하는 방법들을 고려해야합니다.
```
<br>

> ##### 꼬리질문1. DB_CONNECTION 등의 작업을 수행할 때 내부적으로 JDBC api를 구현한 구현체(JDBC Driver)가 필요하다는 말씀이신가요

###### 네! 아래와 같은 작업을 수행할 때 내부적으로 java.sql의 Driver라는 인터페이스를 구현한 구현체가 필요합니다! 

```
Connection connection = DriverManager.getConnection(url, user, password);
```
###### 예컨대 위 작업을 수행할 때 Connection을 얻는 과정에서 아래와 같은 동작을 수행하게 됩니다.

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

###### SQL 인젝션은 어떻게 방지하는건가요?  -> 이거 답변 지금 다 엉터리 다시 볼 것

```
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
> ..
> ..
> ..
>


----
> ###### 꼬리질문 1. Connection Pool은 얼마가 돼야 적당할까요?
```
쓰레드도 고려해야하고, 다양하게 고려해야한다.
```


##### 3. 데이터베이스 clustered index vs non-clustered index에 대해 설명하시오
```
클러스터 인덱스는 마치 전화번호부처럼 데이터 자체를 저장하는 순간부터 물리적으로 정렬해둬서 찾기 편하게 합니다. 근데 이 저장에도 비용이 듭니다.
반면 논 클러스터 인덱스는 검색 조건별로 여러개의 별도 테이블을 둬서 인덱싱 하는 방법인데, 아무래도 별도 테이블에 대해 조인하다 보니 속도가 느립니다.

@ 실제 구현 방법 확인해보기
@ 데이터가 언제 정렬되는지 검토해보기
@ 인덱스를 구분하는 다른 방법들 확인해보기
```
> ..
> ..
> ..


##### 4. 객체 지향 설계 5원칙 - SOLID (vs 다른 개발 원칙)
```

```
> ..
> ..
> ..

