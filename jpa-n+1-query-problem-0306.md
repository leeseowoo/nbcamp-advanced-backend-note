#### JPA N+1 문제
연관 관계가 설정된 엔티티를 조회할 때, 조회된 엔티티 수(N)만큼 연관된 엔티티를 조회하기 위한 여러 개의 조회 쿼리가 발생하는 현상이다.  
게시글과 댓글이 있을 때, 게시글과 댓글은 연관 관계가 설정돼 있을 것이므로 게시글 조회 후 각 게시글마다 댓글까지 조회하기 위한 추가 쿼리가 발생할 수 있다.  
특히 OneToMany 관계에서 자주 발생한다.


#### N+1이라고 부르는 이유
하나의 메인 쿼리(1) 이후 연관된 데이터(N)를 가져오기 위해 추가적인 N개의 쿼리가 실행되기 때문에 'N+1'이라는 이름이 붙었다.
1. 메인 엔티티(부모)를 조회하는 1개의 쿼리 실행
2. 각 부모 엔티티에 대해 연관된 자식 엔티티를 조회하는 N개의 쿼리 추가 실행

```sql
-- 1개의 메인 쿼리 실행 (1)
SELECT * FROM post;

-- 각 Post에 대해 연관된 Comment 조회 (N)
SELECT * FROM comment WHERE post_id = 1;
SELECT * FROM comment WHERE post_id = 2;
SELECT * FROM comment WHERE post_id = 3;
...
```
Post가 100개라면? 총 1 + 100 = 101 개의 SQL 쿼리 실행



#### 해결 방법
1. Fetch Join 사용
2. Entity Graph 사용
3. Batch Size 조정
