## **스프링 트랜잭션 이해**

![image](https://user-images.githubusercontent.com/79301439/210529208-7d2306af-7cdd-415f-a24f-3c6ed2a7d1ae.png)

```java
package hello.springtx.apply;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.transaction.support.TransactionSynchronizationManager;


@Slf4j
@SpringBootTest
public class InternalCallV2Test {

    @Autowired CallService callService;

    @Test
    void printProxy() {
        log.info("callService class={}", callService.getClass());
    }

    @Test
    void externalCallV2() {
        callService.external();
    }

    @TestConfiguration
    static class InternalCallV1TestConfig {

        @Bean
        CallService callService() {
            return new CallService(internalService());
        }

        @Bean
        InternalService internalService() {
            return new InternalService();
        }

    }

    @Slf4j
    @RequiredArgsConstructor
    static class CallService {

        private final InternalService internalService;

        public void external() {
            log.info("call external");
            printTxInfo();
            internalService.internal();
        }

        private void printTxInfo() {
            boolean txActive = TransactionSynchronizationManager.isActualTransactionActive();
            log.info("tx active={}", txActive);
        }
    }

    static class InternalService {

        @Transactional
        public void internal() {
            log.info("call internal");
            printTxInfo();
        }

        private void printTxInfo() {
            boolean txActive = TransactionSynchronizationManager.isActualTransactionActive();
            log.info("tx active={}", txActive);
        }

    }

}
```

![image](https://user-images.githubusercontent.com/79301439/210529437-c5c2b0de-3144-4860-8a76-15f057c01e00.png)

![image](https://user-images.githubusercontent.com/79301439/210529503-64284699-7347-4a5f-8e4a-9913e9f62ab4.png)

![image](https://user-images.githubusercontent.com/79301439/210529562-39aea39b-0e93-4921-b2fd-2799d16476e7.png)

![image](https://user-images.githubusercontent.com/79301439/210529668-bf92fbff-b322-4c7e-b6ea-1df85f106bc7.png)

![image](https://user-images.githubusercontent.com/79301439/210529731-4991ec19-1e23-4c17-87de-d18dbd207b21.png)
