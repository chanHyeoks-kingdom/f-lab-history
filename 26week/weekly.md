[TODO]

1. 자바 reflection의 동작원리와 장단점에 대해 설명하시오


```
public class JsonToJava {
    public static void main(String[] args) throws JsonProcessingException, ClassNotFoundException, InstantiationException, IllegalAccessException {
        String jsonInput = "[{\"eventType\": \"LoginEvent\", \"user\": \"user123\", \"timestamp\": \"2021-01-01T12:00:00\"}," +
                           "{\"eventType\": \"PurchaseEvent\", \"user\": \"user456\", \"amount\": 99.95, \"timestamp\": \"2021-01-01T12:01:00\"}]";

        ObjectMapper mapper = new ObjectMapper();
        JsonNode rootNode = mapper.readTree(jsonInput);

        if (rootNode.isArray()) {
            for (JsonNode node : rootNode) {
                String eventType = node.get("eventType").asText();
                Class<?> eventClass = Class.forName("your.package." + eventType); // 리플렉션을 사용하여 클래스 로드
                Object eventInstance = eventClass.getDeclaredConstructor().newInstance(); // 리플렉션을 사용하여 인스턴스 생성

                Iterator<Map.Entry<String, JsonNode>> fields = node.fields();
                while (fields.hasNext()) {
                    Map.Entry<String, JsonNode> entry = fields.next();
                    String fieldName = entry.getKey();
                    JsonNode fieldValue = entry.getValue();

                    if (!fieldName.equals("eventType")) {
                        Field field = eventClass.getDeclaredField(fieldName); // 리플렉션을 사용하여 필드 접근
                        field.setAccessible(true);
                        if (field.getType().equals(double.class)) {
                            field.set(eventInstance, fieldValue.asDouble());
                        } else {
                            field.set(eventInstance, fieldValue.asText());
                        }
                    }
                }

                System.out.println(eventInstance);
            }
        }
    }
}
```


2. 좋은 개발문화는 무엇인가요? 좋은 개발자란 무엇인가요?에 대한 답변
3. 빅오 표기법(Big-O notation)에 대해 설명하시오
4. 데이터베이스의 정규화와 역정규화에 대해 설명하시오
5. 낙관적 락(Optimistic Lock)과 비관적 락(Pessimistic Lock)에 대해 설명하시오







