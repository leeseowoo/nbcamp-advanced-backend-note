Service에 대한 인터페이스를 만들고 그것을 구현하는 것은 관습일까? Service의 경우 여러 구현체가 생기는 경우는 거의 없을 것 같은데, 그럼에도 불구하고 Service에 대한 인터페이스를 만듦으로써 얻을 수 있는 이점이 무엇일까?

#### 체감했던 이점
메서드가 추가될수록 파일 내용이 길어지면서 동일한 이름을 가진 오버로드된 메서드의 시그니처를 확인하거나 어떤 메서드가 있었는지 구현 클래스에서 확인하기 불편했다. 인터페이스에서는 메서드 시그니처 및 반환 타입만 간결하게 작성돼 있어서 한눈에 확인하기 편했다.


#### Service 인터페이스를 만들 필요가 없는 경우
- 구현체가 하나뿐인 경우
- 변경 가능성이 낮은 경우, 의미 없는 추상화
  - 인터페이스를 만들고 구현체를 만드는 것이 관행적으로 이루어지는 경우가 많은데, 추후 인터페이스를 활용할 계획이 없다면 굳이 필요하지 않음
  - 나중에 구현체가 추가될 수도 있다는 이유만으로 미리 인터페이스를 만드는 건 YAGNI(You Ain’t Gonna Need It) 원칙에 어긋남
#### Service 인터페이스를 만들 필요가 있는 경우
- 여러 구현체가 필요한 경우
  - 카드 결제, 포인트 결제 등 여러 방식이 필요한 경우
    ```java
    public interface PaymentService {
      void processPayment(Order order);
    }
    
    public class CreditCardPaymentService implements PaymentService { ... }
    public class PointPaymentService implements PaymentService { ... }

    ```
- 결합을 느슨하게 만들기 위해
  - 인터페이스 타입을 사용하면 구현 클래스를 쉽게 교체 가능
- 테스트를 쉽게 하기 위해
  - OrderService에서 UserService를 사용하는 상황에서 OrderService를 단위 테스트할 때 UserService를 Mock으로 대체 가능
    ```java
    @Mock
    private UserService userService;
    ```

#### 결론
- 구현체가 하나뿐인 경우 인터페이스 생성 불필요
- 다형성이 필요하거나, 느슨한 결합이 중요한 경우 인터페이스 고려
- 나중에 필요할지도 몰라서라는 이유로 인터페이스를 추가하는 것은 피하기
