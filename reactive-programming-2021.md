Reactive Programming은 데이터의 변화 및 이벤트 흐름을 **비동기**적으로 처리하는 프로그래밍 패러다임이다.

- Reactive Programming은 비동기 데이터 스트림을 효율적으로 처리하는 방법
- RxJava, Project Reactor를 활용하면 네트워크, 데이터베이스, UI 이벤트를 반응형으로 처리 가능
- Spring WebFlux와 함께 사용하면 고성능, 확장 가능한 애플리케이션 개발 가능


#### 반응형으로 처리한다의 의미
반응형(reactive)으로 처리한다는 것은 데이터 흐름이나 이벤트 변화에 즉시 반응하여 처리하는 방식을 의미한다.  
반응형 방식에서는 데이터가 준비되었을 때 콜백을 통해 즉시 반응한다. 즉, **데이터가 준비될 때까지 기다리지 않고 다른 작업을 수행**할 수 있다.

```java
import reactor.core.publisher.Mono;

public Mono<String> getUserName() {
    return database.queryUserNameReactive(); // DB에서 데이터가 준비되면 바로 반응
}
```

- Mono<String>을 반환하여 데이터가 도착하면 즉시 반응하는 방식
- 기다리는 동안 다른 작업을 수행할 수 있어 효율적

```java
public String getUserName() {
    String userName = database.queryUserName(); // DB에서 데이터 가져오기 (Blocking)
    return userName;
}

```

- 일반적인 프로그래밍 방식에서는 한 작업이 끝날 때까지 다음 작업이 대기(blocking)
- 위 코드에서는 database.queryUserName()이 DB에서 응답을 받을 때까지 CPU가 놀고 있어 비효율적
