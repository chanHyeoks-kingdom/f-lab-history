
## 1. 현재 상황
> 프로젝트 첫 주차는 멘토님께서 일반적인 개발사이클과 큰 회사들에서 주로 사용되는 대략적인 아키텍처에 대해서 알려주셨다. 이 내용을 바탕으로 `내가 진행할 프로젝트의 세부적인 내용을 결정`해보는게 `이번 주 해야할 일`이다.
```
나는 시스템이나 애플리케이션등을 설계하는게 익숙하지 않기에 이와 관련된 자료들을 먼저 공부해야 하나 싶었지만 현재 가용할 수 있는 시간이
여유롭지 않아 잘 모르더라도 그냥 해보고 피드백 받는 방향으로 진행키로 했다.
```

## 


<br>

## 2. Attitude
> 정답을 찾는다고 생각하지 말고 내 의견을 먼저 정하고 `어떻게 하면 설득할 수 있는지`를 고민해보자.
```
프로젝트를 시작하면서 주제 선정부터 요구사항, 시나리오 작성등을 고려하게 되면서 `이게 맞는건지 물어봐야 하나?`라는 생각이 자꾸 들었는데
이 태도에 문제가 있음을 깨달았다. 정답을 찾는다고 생각하지 말고 내 의견을 먼저 정하고 `어떻게 하면 설득할 수 있는지`를 고민해보는게 더
나은 선택이라는 결론을 얻었다.
````

##

<br>


## 3. 큰 틀의 목표
> (1) 2티어 CRUD 먼저,
```
1. 동시성 이슈
2. 논-클러스터리드 적용
3. 성능 측정
4. 로깅
5. 모니터링
6. 객체지향스럽게 이벤트 할인률 처리
7. JDBC에서 JPA 도입 해서 다음 문제들이 개선되는 걸 관찰해보기 (지연로딩 캐싱을 이용한 성능개선), (일대다 관계 표현 간결하게), (스키마 변경에 들어가던 LOC 비교)
```

<br>

> (2) 배치
```
1. JVM 설정을 통해 어떤 이점을 가져갈 수 있었는가 
```


> (3) 복습겸 서비스 하나 추가
```-
```

> (4) 인스턴스간 동기화, 문제 해결
```-
```


> (5) 인증 기능 추가
```-
```

> (6) 스프링 부트로 이전
```-
```

## 4. TODO

> 1. 시나리오, 요구사항 필요

> 2. 가상으로 그렸든 진짜 화면이든 그려두기

> 3. 시나리오 기반 유즈케이스 작성. 최소단위, 단순한 CRUD.

> 4. 어떤 서버가 필요한지 검토, (스프링이 되도록). 왜 스프링이 필요한가,

> 5. 스프링 프레임워크 기반 서버 세팅, JDBC로 직접 DB 연동

> 6. 테스트 케이스 먼저 작성, 그거 기준으로 ERD 설계? 그냥 ERD 먼저 설계?

> 7. CRUD API 개발

> 8. 마이크로 서비스니까 주문이면 주문, 상품이면 상품 같이 해야하나? 멘토님 스크립트 다시 보면서 감 잡기

\\ 최대한 단순하게 생각 적게 하고 개발 먼저 해보자,



<br>

# 5. 시나리오 유스케이스

```
1. 상품 관리 서버와 상품 관리자 사이의 useCase

[상품 등록 useCase]
상품 판매 관리자는 상품 관리 페이지에 접근하려면 로그인 해야해, 상품 관리 페이지에선 등록된 상품 조회를 할 수 있고 신규 상품 등록을 할 수 있어. 신규상품 등록시에는 상품관리 서버로부터 해당 상품정보에 대한 유효성 검증을 해야해. 유효성 검증이 통과된 경우 상품을 등록할 수 있어


[고객 문의 사항 조회 useCase]
상품 관리자는 상품 관리 서버를 통해 고객 문의 사항 조회를 할 수 있어. 이 문의 사항은 온라인 쇼핑몰 서버에서 고객들이 실제 작성한 문의 정보들이야. 이곳에 접근하려면 로그인 과정을 거친 뒤 접근할 수 있어


[전체 주문 신청 내역 조회 useCase]
고객이 상품 주문 신청한 내용들을 조회할 수 있는데 이것도 관리자 서버를 통해 제공받을 수 있고 로그인 이후에만 조회할 수 있어


2. 온라인 쇼핑몰 서버와 고객 사이의 useCase

[상품 조회 useCase]
고객은 온라인 쇼핑몰 메인 페이지에 접근해 상품을 조회할 수 있어. 로그인은 하지 않아도 조회는 가능해

[상품 구매 신청 useCase]
고객은 메인 페이지에서 조회 이후 해당 상품을 클릭함으로써 해당 상품의 구매 페이지로 접근할 수 있어, 상품 주문 신청 폼에 맞춰 신청하면 서버로부터 해당 상품의 재고가 있는지 조회하고 재고가 남을시에만 구매신청을 진행할 수 있어. 이 구매 신청은 로그인을 필수로 해야해. 로그인하지 않은 고객은 구매 신청을 눌렀을 시에 로그인 화면으로 넘어가게 돼.

[고객 로그인 useCase]
고객은 메인 페이지 상단의 로그인 버튼이나, 개별 상품 구매 페이지 이동 시도를 통해 로그인을 시도할 수 있어, 먼저 서버로부터 해당 로그인 정보가 있는지 확인하고 있을 시 권한을 인가받게 돼.

[고객 회원가입 useCase]
고객은 로그인 페이지의 회원가입 버튼이나 메인 페이지에서 회원가입 페이지로 넘어갈 수 있어. 이곳에서 회원가입 요청을 할 시엔 서버로부터 입력 값에 대한 유효성 검증을 받고 회원 가입 절차를 진행하게 돼

[고객 민원 등록 useCase]
고객은 로그인 이후 민원을 등록할 수 있어. 메인 페이지에서 고객 주문 내역 확인 및 민원 페이지에 접근해 민원을 작성할 수 있고 민원 등록값 검증 절차를 거쳐 문제가 없을시 민원은 등록되게 돼

[고객 구매 신청 내역 확인 useCase]
고객은 본인이 구매 신청한 내역을 메인 페이지에서  고객 주문 내역 확인 및 민원 페이지에 접근해 확인할 수 있어, 서버로부터 고객ID에 맞는 조회정보를 받게돼
```



<img width="457" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/f0f3bcd5-ec9a-45e7-88dc-8c80442d5387">

<img width="372" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/cd24290c-ea28-4d15-a374-44f8cf09e3dd">



<br>


# 6. 시나리오 와이어 프레임

[상품 관리 로직]
![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/8c8a7e09-2f6b-40ce-8e4a-80ff7b7ffdeb)
![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/01d9a850-1330-46ca-88c5-0af846aa4deb)
![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/60a9370c-d95e-4712-96c8-c1e19b80946f)
![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/4275e92c-3ca1-4b30-9e04-da88f816cb6c)
![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/48f90a56-2410-4f40-b1f3-67178203489e)
![image](https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/eef0adf0-faa5-4928-9628-e906318f4eab)


<br>

[메인 온라인 쇼핑몰 페이지]
<img width="583" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/49f35175-75d0-4544-92c1-821f3e1cec42">
<img width="556" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/2ac0d800-f406-42d5-8ebe-fabe7874a16d">
<img width="723" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/918ebc2d-b938-4e09-b14f-0a834e666fb6">
<img width="590" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/4385a819-364a-4e22-bc59-b49439490afb">
<img width="579" alt="image" src="https://github.com/chanHyeoks-kingdom/f-lab-history/assets/68278903/4883a4ae-5032-4d02-8dfb-7134f6324172">





<br>


# 7. API 명세
# API 설계 문서

## 상품 관리 서버 API

### 1. 관리자 인증 (로그인)

- **POST** `/auth/login`
  - **Request Body**
    ```json
    {
      "username": "admin",
      "password": "password123"
    }
    ```
  - **Response**
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5..."
    }
    ```

### 2. 등록된 상품 조회

- **GET** `/products`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Response**
    ```json
    [
      {
        "id": 1,
        "name": "상품명",
        "description": "상품 설명",
        "price": 10000
      }
    ]
    ```

### 3. 신규 상품 등록 및 유효성 검증

- **POST** `/products`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Request Body**
    ```json
    {
      "name": "새 상품",
      "description": "상품 설명",
      "price": 20000
    }
    ```
  - **Response**
    ```json
    {
      "success": true,
      "id": 2
    }
    ```

### 4. 고객 문의 사항 조회

- **GET** `/customer/inquiries`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Response**
    ```json
    [
      {
        "id": 1,
        "customerName": "고객명",
        "inquiry": "문의 내용"
      }
    ]
    ```

### 5. 전체 주문 신청 내역 조회

- **GET** `/orders`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Response**
    ```json
    [
      {
        "orderId": 1,
        "customerName": "고객명",
        "orderedProducts": [
          {
            "productId": 1,
            "quantity": 2
          }
        ],
        "totalPrice": 20000
      }
    ]
    ```

## 온라인 쇼핑몰 서버 API

### 1. 상품 조회

- **GET** `/products`
  - **Response**
    ```json
    [
      {
        "id": 1,
        "name": "상품명",
        "description": "상품 설명",
        "price": 10000
      }
    ]
    ```

### 2. 상품 구매 신청 및 재고 확인

- **POST** `/orders`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Request Body**
    ```json
    {
      "productId": 1,
      "quantity": 1
    }
    ```
  - **Response**
    ```json
    {
      "success": true,
      "orderId": 1
    }
    ```

### 3. 고객 인증 (로그인)

- **POST** `/auth/login`
  - **Request Body**
    ```json
    {
      "email": "customer@example.com",
      "password": "password123"
    }
    ```
  - **Response**
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5..."
    }
    ```

### 4. 회원가입 및 유효성 검증

- **POST** `/auth/register`
  - **Request Body**
    ```json
    {
      "email": "new@example.com",
      "password": "newpassword123",
      "name": "고객명"
    }
    ```
  - **Response**
    ```json
    {
      "success": true
    }
    ```

### 5. 고객 민원 등록

- **POST** `/customer/inquiries`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Request Body**
    ```json
    {
      "inquiry": "문의 내용"
    }
    ```
  - **Response**
    ```json
    {
      "success": true,
      "inquiryId": 1
    }
    ```

### 6. 구매 신청 내역 조회

- **GET** `/orders/my`
  - **Headers**
    ```
    Authorization: Bearer <token>
    ```
  - **Response**
    ```json
    [
      {
        "orderId": 1,
        "orderedProducts": [
          {
            "productId": 1,
            "quantity": 2
          }
        ],
        "totalPrice": 20000
      }
    ]
    ```










<br>
<br>
<br>




#### Q1. 마이크로 서비스가 무엇인가,

