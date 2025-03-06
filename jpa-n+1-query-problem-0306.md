#### JPA N+1 문제
연관 관계가 설정된 엔티티를 조회할 때, 조회된 엔티티 수(N)만큼 연관된 엔티티를 조회하기 위한 여러 개의 조회 쿼리가 발생하는 현상이다.  
게시글과 댓글이 있을 때, 게시글과 댓글은 연관 관계가 설정돼 있을 것이므로 게시글 조회 후 각 게시글마다 댓글까지 조회하기 위한 추가 쿼리가 발생할 수 있다.  
특히 OneToMany 관계에서 자주 발생한다.


#### N+1이라고 부르는 이유
하나의 메인 쿼리(1) 이후 연관된 데이터(N)를 가져오기 위해 추가적인 N개의 쿼리가 실행되기 때문에 'N+1'이라는 이름이 붙었다.
1. 메인 엔티티(부모)를 조회하는 1개의 쿼리 실행
2. 각 부모 엔티티에 대해 연관된 자식 엔티티를 조회하는 N개의 쿼리 추가 실행

```java
List<Post> posts = em.createQuery("SELECT p FROM Post p", Post.class)
                     .getResultList();

for (Post post : posts) {
    System.out.println(post.getComments().size()); // 댓글 조회 (N번 실행)
}
```

```sql
-- 1개의 메인 쿼리 실행 (1)
SELECT * FROM post;

-- 각 Post에 대해 연관된 Comment 조회 (N)
SELECT * FROM comment WHERE post_id = 1;
SELECT * FROM comment WHERE post_id = 2;
SELECT * FROM comment WHERE post_id = 3;
...
```

Post가 100개라면? 총 1 + 100 = **101 개의 SQL 쿼리** 실행



#### findAll 메서드를 사용할 때, 글로벌 패치 전략에 따라 N+1 문제가 어떻게 발생하는가
- **글로벌 패치 전략 - 즉시 로딩**
즉시 로딩으로 설정하고 findAll()을 실행하면 N + 1 문제가 발생한다.
findAll()은 `select u from User u`라는 JPQL 구문을 생성해서 실행하기 때문이다. **JPQL은 글로벌 패치 전략을 고려하지 않고 쿼리를 실행**한다. 모든 User를 조회하는 쿼리 실행 후에 즉시 로딩 설정을 보고 연관 관계에 있는 모든 엔티티를 조회하기 위한 여러 개의 쿼리를 추가적으로 실행한다.

- **글로벌 패치 전략 - 지연 로딩**
지연 로딩으로 설정하고 findAll()을 실행하면 N + 1 문제가 발생하지 않는다. 연관 관계에 있는 엔티티를 실제 객체 대신 프록시 객체로 생성하여 주입하기 때문이다. 하지만 프록시 객체를 사용할 경우 실제 데이터가 필요하여 조회하는 쿼리가 발생하고 이때 N + 1 문제가 발생 가능하다.


#### 해결 방법
1. **Fetch Join 사용**
```java
List<Post> posts = em.createQuery(
    "SELECT p FROM Post p JOIN FETCH p.comments", Post.class)
    .getResultList();
```
**한 번의 쿼리**로 모든 데이터를 가져온다.
```sql
SELECT p.*, c.* FROM post p 
LEFT JOIN comment c ON p.id = c.post_id;
```


2. **Entity Graph 사용**
```java
@EntityGraph(attributePaths = {"posts"}, type = EntityGraphType.FETCH)
List<User> findAll();
```


4. **Batch Size 조정**
```java
@Entity
@BatchSize(size = 10)
public class Post { ... }
```
