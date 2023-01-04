## **스프링 트랜잭션 이해**

![image](https://user-images.githubusercontent.com/79301439/210522570-55ee84ea-a7b9-4447-8b02-2bc87594b841.png)

![image](https://user-images.githubusercontent.com/79301439/210522682-bc38370b-fcba-4d13-b6f7-944cff3a3860.png)

```java
package hello.springtx.apply;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.aop.support.AopUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.transaction.support.TransactionSynchronizationManager;

import static org.assertj.core.api.Assertions.assertThat;

@Slf4j
@SpringBootTest
public class InternalCallV1Test {

    @Autowired CallService callService;

    @Test
    void printProxy() {
        log.info("callService class={}", callService.getClass());
        assertThat(AopUtils.isAopProxy(callService)).isTrue();
    }

    @Test
    void internalCall() {
        log.info("주입된 인스턴스 class={}", callService.getClass());
        callService.internal();
    }

    @Test
    void externalCall() {
        log.info("주입된 인스턴스 class={}", callService.getClass());
        callService.external();
    }

    @TestConfiguration
    static class InternalCallV1TestConfig {

        @Bean
        CallService callService() {
            return new CallService();
        }

    }

    @Slf4j
    static class CallService {

        public void external() {
            log.info("call external");
            printTxInfo();
            internal();
        }

        @Transactional
        public void internal() {
            log.info("call internal");
            log.info("호출된 인스턴스 class={}", this.getClass());
            printTxInfo();
        }

        private void printTxInfo() {
            boolean txActive = TransactionSynchronizationManager.isActualTransactionActive();
            log.info("tx active={}", txActive);
        }
    }

}
```

![image](https://user-images.githubusercontent.com/79301439/210522932-b534045b-2ca3-4f2d-a7f8-deb897977e84.png)

![image](https://user-images.githubusercontent.com/79301439/210523195-7d509e3c-ca2d-4b25-9c95-cdce5be2dc0b.png)

![image](https://user-images.githubusercontent.com/79301439/210523266-f8ff955b-9e0e-448e-9cfb-06984b228ecc.png)

![image](https://user-images.githubusercontent.com/79301439/210523430-bbbf4c46-0f13-4481-b01f-606c20359746.png)

![image](https://user-images.githubusercontent.com/79301439/210523488-e38c3f98-7709-4014-84f9-6ae5d8a5a35b.png)

![image](https://user-images.githubusercontent.com/79301439/210523698-37a5304c-6b00-4f8c-907b-a78ed2d162c0.png)

![image](https://user-images.githubusercontent.com/79301439/210523771-64ca76d2-742b-4055-8da3-cc92fd7d114b.png)
