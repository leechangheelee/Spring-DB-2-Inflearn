## **스프링 트랜잭션 전파1 - 기본**

![image](https://user-images.githubusercontent.com/79301439/211304495-4b5064c7-01c5-44bc-b566-f0b7ca40fee0.png)

![image](https://user-images.githubusercontent.com/79301439/211304584-41d37368-c592-4dd9-ad76-f3f810ac873d.png)

![image](https://user-images.githubusercontent.com/79301439/211304645-a8e2cf40-87b2-42a7-a001-324081858a25.png)

```java
import org.springframework.transaction.TransactionDefinition;

@Test
void inner_rollback_requires_new() {
    log.info("외부 트랜잭션 시작");
    TransactionStatus outer = txManager.getTransaction(new DefaultTransactionAttribute());
    log.info("outer.isNewTransaction()={}", outer.isNewTransaction()); //true

    log.info("내부 트랜잭션 시작");
    DefaultTransactionAttribute definition = new DefaultTransactionAttribute();
    definition.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRES_NEW);
    TransactionStatus inner = txManager.getTransaction(definition);
    log.info("inner.isNewTransaction()={}", inner.isNewTransaction()); //true

    log.info("내부 트랜잭션 롤백");
    txManager.rollback(inner); //롤백

    log.info("외부 트랜잭션 커밋");
    txManager.commit(outer); //커밋
}
```

![image](https://user-images.githubusercontent.com/79301439/211304804-f6b1c658-4cfd-4ac3-a91f-342b716b1c92.png)

![image](https://user-images.githubusercontent.com/79301439/211305074-f43e0f57-cb6d-4e45-a7f5-b60124956fbb.png)

![image](https://user-images.githubusercontent.com/79301439/211305179-5a6e7e65-80bc-42a6-a101-7df1db66bd8a.png)

![image](https://user-images.githubusercontent.com/79301439/211305219-ad8de19f-473e-4bca-82fc-e033ab30b91c.png)

![image](https://user-images.githubusercontent.com/79301439/211305304-400ed2f0-8322-4dc6-b38f-0f369f04dac0.png)

![image](https://user-images.githubusercontent.com/79301439/211305418-097a109e-bad8-4b5d-8ac9-50d2957649ea.png)

![image](https://user-images.githubusercontent.com/79301439/211305461-8635e1db-2001-4684-9e69-a3f18f0662f4.png)

![image](https://user-images.githubusercontent.com/79301439/211305500-e40bf804-a812-44aa-ae8a-f1a551decde7.png)

![image](https://user-images.githubusercontent.com/79301439/211305620-fcc3c3af-d022-475a-bc16-b6cbd3968cad.png)

![image](https://user-images.githubusercontent.com/79301439/211305648-2a215798-aec6-427c-8589-42954b879746.png)
