<img width="463" alt="image" src="https://github.com/user-attachments/assets/67209a01-1bc4-473a-a17d-8937f0c14240">

리플렉션의 단점?, 성능상의 문제 등 .. 추가 질문 고민하기

```
[질문 리스트]

자바 reflection의 동작원리와 장단점에 대해 설명하시오
좋은 개발문화는 무엇인가요? 좋은 개발자란 무엇인가요?에 대한 답변
빅오 표기법(Big-O notation)에 대해 설명하시오
데이터베이스의 정규화와 역정규화에 대해 설명하시오
```


## Q1. 자바 reflection의 동작원리와 장단점에 대해 설명하시오

> A. 자바 리플렉션의 동작 원리

`자바 리플렉션은` 런타임 환경에서 클래스, 메소드, 필드, 생성자등의 런타임 환경에서 일시적으로 클래스 정보들을 조작할 수 있도록 하는 매커니즘을 제공함
이런 클래스 정보들은 JVM의 메서드 영역에 저장되어 있는 것들을 가져오는 것이고 이를 통해 `코드 컴파일 시점이 아니라 런타임 시점에 클래스 정보를 변경할 수 있다`


- ### 예시 케이스
```
public class Human {

    private String name;

    public Human(String name){
        this.name = name;
    }

    private Human(){

    }

    public void goRestRoom(){
        System.out.println(name +"이 화장실로 갑니다.");
    }

    public void offPants(){
        System.out.println(name +"이 바지를 내립니다.");
    }

    public void doWork(){
        System.out.println(name + "이 볼일을 봅니다.");
        poopOut();
    }

    private void poopOut(){
        System.out.println("똥이 나왔습니다.");
    }
}
```

```
Class<?> class1 = Class.forName("org.example.reflection.Human");

Constructor<?> constructor = class1.getConstructor(String.class);
Object human = constructor.newInstance("승갱이");

Method goRestRoomMethod = class1.getDeclaredMethod("goRestRoom");
Method offPantsMethod = class1.getDeclaredMethod("offPants");
Method doWorkMethod = class1.getDeclaredMethod("doWork");
Method poopOutMethod = class1.getDeclaredMethod("poopOut");

poopOutMethod.setAccessible(true);
poopOutMethod.invoke(human);
goRestRoomMethod.invoke(human);
offPantsMethod.invoke(human);
doWorkMethod.invoke(human);
```

![image](https://github.com/user-attachments/assets/e92aa41e-64d2-47aa-a1f3-e026047bc124)


<br>
<br>

위 예시코드를 보면 private로 정의되어있던 똥싸기를 동적으로 변경해 외부에서 호출토록 했다. 덕분에 화장실 가기 전에 똥을 쌀 수 있게 됐다.
이제 좀 더 실제적인 예시인 JSON Mapper와 DI의 예시를 보자.


<br> 

- ### JSON 예시
```
import java.lang.reflect.Field;
import org.json.JSONObject;  // org.json 라이브러리를 사용

public class JsonToJavaReflection {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\", \"age\":30}";
        JSONObject jsonObject = new JSONObject(jsonStr);
        User user = new User();

        try {
            for (Field field : User.class.getDeclaredFields()) {
                field.setAccessible(true);  // private 필드에 접근 가능하도록 설정
                String fieldName = field.getName();
                Object value = jsonObject.get(fieldName);

                // 필드 타입에 맞게 값을 변환하고 할당
                if (field.getType() == int.class) {
                    field.setInt(user, (Integer) value);
                } else if (field.getType() == String.class) {
                    field.set(user, (String) value);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        // 결과 출력
        System.out.println("User Name: " + user.getName());
        System.out.println("User Age: " + user.getAge());
    }
}
```

위 처럼 JsonMapper를 만들려면 결국 `특정 필드의 private 설정을 무시하고 조회, 값 저장을 할 수 있어야 한다`던가 어떤 필드에 값을 세팅해야 하는지 해당 필드 명들을 받아올 수 있어야한다. 이 때 리플렉션이라는 개념이 필요하다.

* 결국 리플렉션은 클래스 정보를 실제 실행 환경에서 변경하기 위해 수행하는 조회, 수정 모든 작업을 포괄하는 개념이다.



<br> 

- ### DI FrameWork
```
# 먼저 인터페이스 상속
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface SSKAutowired {
}
```

```
# DI를 적용할 클래스
public class Robot {

    public void fight(){
        System.out.println("로봇이 싸웁니다.");
    }

    public void clean(){
        System.out.println("로봇이 청소합니다.");
    }

    private void destroy(){
        System.out.println("로봇이 파괴됩니다.");
    }
}
```

```
# 주입 받아보기
public class TestService {

    @SSKAutowired
    private Robot robot;

    public Robot getRobot(){
        return robot;
    }
    public void start(){
        robot.fight();
        robot.clean();
    }
}
```


```
public class CustomApplicationContext {

    /**
     * 클래스의 멤버필드 중 SSKAutowired가 붙어있을 경우 의존성 주입
     * @param clazz - 스캔 클래스
     * @return - 의존주입이 완료된 스캔 클래스
     * @throws Exception
     */
    public static <T> T getInstance(Class<T> clazz) throws Exception{

        T instance = createInstance(clazz);
        Arrays.stream(clazz.getDeclaredFields()).forEach(field -> {
            if(field.getAnnotation(SSKAutowired.class) != null){ // SSKAutowired가 붙은 멤버필드일 경우
                try {
                    Object fieldInstance = createInstance(field.getType()); // 멤버필드에 대한 객체 생성
                    field.setAccessible(true);
                    field.set(instance, fieldInstance); // 생성된 객체를 instance에 셋팅 (DI)
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }
        });
        return instance;
    }

    /**
     * 리플렉션 기본 생성자를 통해 객체 생성
     * @param clazz - 클래스 타입
     * @return 클래스 객체
     * @throws Exception
     */
    private static <T> T createInstance(Class<T> clazz) throws Exception{
        Constructor<T> constructor = clazz.getDeclaredConstructor(); // 리플렉션을 통해 클래스의 기본생성자 정보 조회
        constructor.setAccessible(true);
        return constructor.newInstance(); // 객체 생성
    }
}
```

```
@Test
void getInstance() throws Exception {

    TestService testService = CustomApplicationContext.getInstance(TestService.class);

    assertNotNull(testService.getRobot());
    testService.start();
}
```








