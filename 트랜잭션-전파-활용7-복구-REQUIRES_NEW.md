## **스프링 트랜잭션 전파2 - 활용**

![image](https://user-images.githubusercontent.com/79301439/211334188-1e8bdb66-4b08-4dc2-97ce-2f58144224a3.png)

```java
/**
 * memberService    @Transactional:ON
 * memberRepository @Transactional:ON
 * logRepository    @Transactional:ON(REQUIRES_NEW) Exception
 */
@Test
void recoverException_success() {
    //given
    String username = "로그예외_recoverException_success";

    //when
    memberService.joinV2(username);

    //then: member 저장, log 롤백
    assertTrue(memberRepository.find(username).isPresent());
    assertTrue(logRepository.find(username).isEmpty());
}
```

![image](https://user-images.githubusercontent.com/79301439/211334316-f9f70003-7c18-4a0c-892a-fb38c47c0d56.png)

![image](https://user-images.githubusercontent.com/79301439/211334373-7f833ca0-740b-444d-9b24-d22cb9ef4e46.png)

![image](https://user-images.githubusercontent.com/79301439/211334425-9b07d71c-92f4-41e7-9f72-69afa10405f4.png)

![image](https://user-images.githubusercontent.com/79301439/211334468-8447a80a-e2fb-4eaa-9d40-ea3f83d44822.png)

![image](https://user-images.githubusercontent.com/79301439/211334539-117d6ce3-3c7e-42f4-90f3-1edf082588e8.png)

![image](https://user-images.githubusercontent.com/79301439/211334582-fd60fe46-4894-4ca9-a734-9a1bce8b6f1f.png)

![image](https://user-images.githubusercontent.com/79301439/211334685-58682f11-6433-40d5-b942-20c10dd18aca.png)

![image](https://user-images.githubusercontent.com/79301439/211334773-5b404ed2-3a8b-4d63-bc2b-c40a1dd859c6.png)

![image](https://user-images.githubusercontent.com/79301439/211334910-e7fa071a-837f-467d-87ab-90131572273d.png)

![image](https://user-images.githubusercontent.com/79301439/211334964-5dbd2f35-0801-4c28-8b83-7bcffd1ecad4.png)
