## **스프링 트랜잭션 전파2 - 활용**

![image](https://user-images.githubusercontent.com/79301439/211330318-84267f38-f051-4fde-bb9a-4d8073df15d5.png)

![image](https://user-images.githubusercontent.com/79301439/211330380-bc66b98e-7708-46d2-83a3-20623ea60937.png)

```java
/**
 * memberService    @Transactional:ON
 * memberRepository @Transactional:ON
 * logRepository    @Transactional:ON Exception
 */
@Test
void recoverException_fail() {
    //given
    String username = "로그예외_recoverException_fail";

    //when
    assertThatThrownBy(() -> memberService.joinV2(username))
            .isInstanceOf(UnexpectedRollbackException.class);

    //then: 모든 데이터가 롤백된다.
    assertTrue(memberRepository.find(username).isEmpty());
    assertTrue(logRepository.find(username).isEmpty());
}
```

![image](https://user-images.githubusercontent.com/79301439/211330546-6aab3164-5800-4cab-a7c4-75849ae3095d.png)

![image](https://user-images.githubusercontent.com/79301439/211330622-f842e8c0-b3a9-4853-bea7-8388ccdf6b94.png)

![image](https://user-images.githubusercontent.com/79301439/211330687-fae92e2f-bf85-4b0f-bd6e-e5e4edf606c9.png)

![image](https://user-images.githubusercontent.com/79301439/211330770-193c151c-2d95-4d0d-872f-fba9e3248a95.png)
