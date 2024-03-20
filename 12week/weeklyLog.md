# WEEK 11

```
[이 주의 과제]
1. OpenAPI spec과 사용 이유
2. Rest API 설계 원칙과 네이밍 컨벤션
3. MSA(마이크로 서비스 아키텍처)에 대해 설명하시오
4. Docker/Containerization에 대해 설명하시오
```

## [1. 자문자답]


----------


### 1. OpenAPI spec과 사용 이유


#### (1) OpenAPI spec이 뭐고 왜 써주는거예요?
> HTTP 기반의 API 명세를 위한 표준입니다. OAS 기반의 명세들은 주로 JSON이나 XML로 작성되는데 개발자들은 이를 활용해 빠르게 해당 API를 이해할 수 있게됩니다.
```

```

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. OAS(OpenApiSepcificate) 기반의 문서를 처음부터 직접 작성하는 건 어렵다고 들었는데, 일반적으론 어떻게 작성하나요?

```
네 물론 말씀하신 것 처럼 처음부터 직접 작성할 수도 있지만 이는 상황에 따라 번거로울 수 있습니다. 그래서 일반적으로 SwaggerHub나 SwaggerEdit를 이용해 쉽게 작성합니다.
이런 도구들은 정의된 API를 자동으로 문서화 해주고 실시간으로 테스트할 수 있는 UI들을 제공해줍니다.
```

</details>

<details>
<summary> 꼬리질문 2. </summary>

###### 꼬리질문 2. 실제 사용 예시를 보여주세요

```
[관련 애토네이션]
@Api: 클래스 수준에서 사용되어, 해당 클래스가 Swagger 리소스임을 명시합니다.
@ApiOperation: 메소드 수준에서 사용되어, 개별 API 연산에 대한 설명을 제공합니다.
@ApiParam: 메소드의 매개변수에 사용되어, 파라미터에 대한 메타데이터(예: 설명, 필수 여부 등)를 제공합니다.
@ApiResponse: 메소드 수준에서 사용되어, API 응답에 대한 상세 정보를 명시합니다.
@ApiModel: 모델 클래스에 사용되어, 모델에 대한 설명을 제공합니다.
@ApiModelProperty: 모델 속성에 사용되어, 속성에 대한 설명과 추가 메타데이터를 제공합니다.
```

##### [useCase]
```
// 의존성
implementation 'io.springfox:springfox-boot-starter:3.0.0'
```

```
@Configuration
@EnableSwagger2
public class SwaggerConfig {                                    
    @Bean
    public Docket api() { 
        return new Docket(DocumentationType.SWAGGER_2)  
          .select()                                  
          .apis(RequestHandlerSelectors.any())              
          .paths(PathSelectors.any())                          
          .build();                                           
    }
}
```

```
@Api(value = "User Management System", description = "Operations pertaining to user in User Management System")
@RestController
public class UserController {

    @ApiOperation(value = "View a list of available users", response = List.class)
    @GetMapping("/users")
    public String getUsers(@ApiParam(value = "Page number for user list pagination", required = true) @RequestParam(value = "page") int page) {
        return "List of Users for page: " + page;
    }
}
```

```
http://localhost:8080/swagger-ui/
```

</details>



<details>
<summary> 꼬리질문 3. </summary>

###### 꼬리질문 3. OpenApi 명세를 작성할 때 JSON과 YAML을 사용하는거로 알고 있는데 두 포맷간의 차이점과 YAML을 선호하는 사람들이 있는 이유가 궁금합니다.
```
JSON의 경우 중괄호, :, 띄어쓰기를 통해 데이터를 표현하는 형식입니다. 반면 YAML은 들여쓰기를 이용해 표현하는데 이런 부분이 사람이 읽기 더 좋다고 평가받고 있어서
많은 사람들에게 선호되는 경향이 있는 거 같습니다.
```

</details>

<details>
<summary> 꼬리질문 4. </summary>

###### 꼬리질문 4. 자동으로 생성된 클라이언트 라이브러리나 서버 스텁을 실제 프로젝트에서 사용해도 괜찮은가요?
```
JSON의 경우 중괄호, :, 띄어쓰기를 통해 데이터를 표현하는 형식입니다. 반면 YAML은 들여쓰기를 이용해 표현하는데 이런 부분이 사람이 읽기 더 좋다고 평가받고 있어서
많은 사람들에게 선호되는 경향이 있는 거 같습니다.
```

</details>

<details>
<summary> 꼬리질문 5. </summary>

###### 꼬리질문 5. OpenAPI 문서를 업데이트하는 베스트 프랙티스는 무엇인가요?
```
JSON의 경우 중괄호, :, 띄어쓰기를 통해 데이터를 표현하는 형식입니다. 반면 YAML은 들여쓰기를 이용해 표현하는데 이런 부분이 사람이 읽기 더 좋다고 평가받고 있어서
많은 사람들에게 선호되는 경향이 있는 거 같습니다.
```

</details>




----------


### 2. Rest API 설계 원칙과 네이밍 컨벤션


#### (1) Rest API 설계 원칙이 뭐고 네이밍 컨벤션은 어떤거예요?
> ##### 그르게요 ..
```
인생 참 ..
```

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 근데요 .. 그렇게 보면 

```
.. 한 거 아닌가요?
```

</details>


----------


### 3. MSA(마이크로 서비스 아키텍처)에 대해 설명하시오


#### (1) MSA(마이크로 서비스 아키텍처)이 궁금해요!
> ##### 그르게요 ..
```
인생 참 ..
```

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 근데요 .. 그렇게 보면 

```
.. 한 거 아닌가요?
```

</details>


----------


### 4. Docker/Containerization에 대해 설명하시오


#### (1) 도커가 뭐고 컨테이너화는 뭐예요?
> ##### 그르게요 ..
```
인생 참 ..
```

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 근데요 .. 그렇게 보면 

```
.. 한 거 아닌가요?
```

</details>








