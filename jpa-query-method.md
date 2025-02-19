#### 'findById~'의 Id 부분에 Cannot resolve property 'id'라는 오류 발생 원인  
Spring Data JPA에서 기본 제공되는 'findById'와 같은 쿼리 메서드의 경우, JPA가 자동으로 엔티티의 @Id 필드를 찾아서 조회하기 때문에 필드명이 id가 아니어도 @Id가 붙은 필드를 기준으로 정상 동작한다.  
그러나 'findByIdAndDeletedAtIsNullAndHiddenFalse'와 같은 사용자 정의 쿼리 메서드의 경우, 필드명을 직접 해석해야 하는데 엔티티에 id 필드가 없으면 오류가 발생한다. (Product 엔티티의 id를 productId라고 사용하는 경우)  
해결하려면 엔티티 필드명을 확인하고, 쿼리 메서드에서 정확한 필드명을 사용하면 된다. 'findByProductIdAndDeletedAtIsNullAndHiddenFalse'
