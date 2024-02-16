

# WEEK 7

```
[이 주의 과제]
1. 디자인 패턴 중 3가지 선택- 예: 싱글톤, 팩토리, 어댑터 (디자인 패턴 사용 이유, 예제, 조심해야 할 점)
2. 스프링의 IoC(Inversion of Control)과 DI(Dependency Injection)에 대해 설명하시오
3. 스프링 빈 주입 방법에 대해 설명하시오
4. Spring AOP(Aspect-Oriented Programming)에 대해 설명하시오
5. Git Branch 전략에 대해 설명하시오 (eg. git flow, github flow, etc)
```



## [자문자답]


----------


### 1. 디자인 패턴 중 3가지 선택- 예: 싱글톤, 팩토리, 어댑터 (디자인 패턴 사용 이유, 예제, 조심해야 할 점)
> ##### 오늘 말씀드릴 3가지 디자인 패턴은 팩토리 패턴은 `구체적인 것에 의존하지 말고 추상적인 것에 의존하라`라는 원리를 가집니다?
```
class Main{
  private static final String inputCarName = "미니붕붕이";
  public void main(String[] hyeok){
      Car car = null;

      // TODO 1. 차 이름에 맞는 제조사의 클래스 생성
      if (inputCarName == "소나타" || inputCarName == "뭐시깽이현대차2"){
          car = new HyundaiCar(inputCarName);
      } else if (inputCarName == "미니쿠퍼"){
          car = new SamsungCar(inputCarName);
      } else if (inputCarName == "테슬라 모델X" || inputCarName == "테슬라 모델Y" || inputCarName == "테슬라 모델Z"){
          car = new TeslaCar(inputCarName);
      }
  }
  ...
}
```
```
abstract class Car { .. }
```
```
# 예컨대 아래 같은 각 벤더에 맞는 차량 제조사 클래스가 있다고 가정
class HyundaiCar extends Car{
    private String carName;
    private int carNumber;
    public hyundaiCar(String carName) {
      this.carName = carName;
    }
}
```
<br>

##### 위 코드에서 main 클래스에 객체 생성과 관련된 문맥들이 있다보니 실제 처리하고자 하는 비즈니스 로직을 보기 어렵게 만들 수 있다. 그래서 저런 `객체 생성 관련 로직`은 `팩토리 패턴` 이라는 걸 이용하면 좋다. 아래 팩토리 클래스를 만들어 옮겨보자 ..


```
class CarFactory{
  ..

}

```




<br>

### 2. 스프링의 IoC(Inversion of Control)과 DI(Dependency Injection)에 대해 설명하시오
> Ioc라는 건 ..
```

```
<br>

### 3. 스프링 빈 주입 방법에 대해 설명하시오
> Bean을 주입하는 방법은 ..
```

```
<br>

### 4. Spring AOP(Aspect-Oriented Programming)에 대해 설명하시오
> 먼저 AOP 라는 건 ..
```

```
<br>

### 5. Git Branch 전략에 대해 설명하시오 (eg. git flow, github flow, etc)
> Git branch는 ..
```

```
<br>
