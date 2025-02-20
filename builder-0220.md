#### @Builder(toBuilder = true)  
toBuilder = true 옵션은 기존 객체를 수정하여 새로운 객체를 생성할 수 있도록 한다.

```java
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder(toBuilder = true)  // toBuilder 옵션 추가
public class Product {
    private String name;
    private double price;
}
```

```java
Product product = Product.builder()
    .name("Laptop")
    .price(1200.0)
    .build();

Product updatedProduct = product.toBuilder()  // 기존 객체를 기반으로 빌더 시작
    .price(1500.0)  // 가격 변경
    .build();

System.out.println(product.getPrice());  // 1200.0
System.out.println(updatedProduct.getPrice());  // 1500.0

```

#### @Builder(toBuilder = true)를 언제 사용할까?
- 불변 객체(Immutable Object)를 유지하면서 일부 필드만 변경하고 싶을 때
- DTO, Entity 등에서 특정 값만 수정하는 경우
- JPA 엔티티 업데이트 시 기존 객체를 복사하여 새로운 객체를 생성하는 경우

#### 요약
- @Builder → 객체 생성을 위한 빌더 패턴 제공
- @Builder(toBuilder = true) → 기존 객체를 기반으로 일부 필드만 변경하는 새로운 객체 생성 가능
- 불변성을 유지하면서 필드 변경이 필요한 경우 유용하게 활용 가능
