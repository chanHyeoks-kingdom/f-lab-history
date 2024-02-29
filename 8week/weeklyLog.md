# WEEK 8

```
[이 주의 과제]
1. Git Branch 전략에 대해 설명하시오 (eg. git flow, github flow, etc)
2. ORM (Object Relational Mapping)에 대해 설명하시오
3. ORM/JPA의 n+1문제와 해결방법에 대해 설명하시오
4. 스프링 JPA 영속성 컨텍스트란? (EntityManger, Entity 생명주기, 1차 캐시, 쓰기 지연, dirty checking, flush)
```



## [자문자답]


----------


### 1. Git Branch 전략에 대해 설명하시오 (eg. git flow, github flow, etc)


#### (1) Git flow는 뭐예요?
> ##### 
```
Git flow는 main, develop, feature, release, hotfix로 이루어진 Git을 잘 이용하기 위한 전략입니다!
main에는 현재 버전, develop은 main을 복제한 뒤 신기능 관련된 개발을 진행하는 브랜치입니다! 여기에 직접 코드를 push하면 문제가 생겼을 때 관리가 복잡해질 수 있기 때문에 develop을 체크아웃 해서 각각의 feature 브랜치를 만들어 작업 하고 그 다음에 develop 브랜치에
합치는 방식을 권고합니다. 이렇게 develop에서 대략적인 개발이 완료되면 출시를 위해 main에 merge해도 되지만 안전성을 위해 realease라는 임시 브랜치를 만들어서 테스트나 QA 이후에 main에 합칩니다. 단, release가 끝날 때 Develop 브랜치에도 해당 릴리즈 버전의 코드를
합쳐주어야합니다. 만약 운영에서 발생한 긴급한 문제를 해결할 때 이런 단계는 번거로울 수 있기 때문에 메인에서 hotfix라는 브랜치를 바로 체크아웃 해 작업 후 main에 머지합니다. 이 과정 역시 develop나 현재 진행중인 release에 병합해줘야 합니다.

이게 GitFlow입니다.
```

#### (2) Github flow는 뭐예요?
> ##### 깃허브에서 만든 비교적 단순한 브랜치 전략입니다. 브랜치 전략은 하나의 저장소를 여러 사람이 어떻가 하면 잘 쓸까라는 방법을 말하는데요, githubflow는 이에 대한 해결책으로 특정 작업에 대한 별도 브랜치 하나를 활용하는 방법을 제안하는 전략입니다.
```


```

<br>

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 실제 사용 사례가 어떤게 있나요?

```
.. 상세 설명
```

</details>



----------


### 2. ORM (Object Relational Mapping)에 대해 설명하시오

#### (1)ORM이 뭐예요?
> ##### 객체지향이랑 관계형 데이터베이스를 매핑해주자는 개념입니다! 예를 들어 자바 진영의 JPA라는 인터페이스를 구현한 하이버네이트라는 구현체가 있습니다!
```
JPA라는 인터페이스를 구현한 하이버네이트는 ..

```

<br>

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 실제 사용 사례가 어떤게 있나요?

```
.. 상세 설명
```

</details>



----------


### 3. ORM/JPA의 n+1문제와 해결방법에 대해 설명하시오

#### (1) 혹시 JPA를 써보셨으면 n+1 문제 같은 걸 경험해보신 적이 있으실까요?
> ##### n + 1 문제는 주로 ~ 전략에 의해서, 해결을 위해선 지연 로딩이 어쩌고 ..
```
상세 설명

```

<br>

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 실제 사용 사례가 어떤게 있나요?

```
.. 상세 설명
```

</details>



----------


### 4. 스프링 JPA 영속성 컨텍스트란? (EntityManger, Entity 생명주기, 1차 캐시, 쓰기 지연, dirty checking, flush)

#### (1) 영속성 컨텍스트가 뭐예요?
> ##### JPA는 영속성 컨텍스트라는걸 ..
```
상세 설명

```

<br>

<details>
<summary> 꼬리질문 1. </summary>

###### 꼬리질문 1. 실제 사용 사례가 어떤게 있나요?

```
.. 상세 설명
```

</details>



