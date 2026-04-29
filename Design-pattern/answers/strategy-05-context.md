## Q. 컨텍스트(Context)란 무엇이며, 전략 패턴에서 어떤 역할을 하나요?

### 핵심 답변
Context는 전략 패턴에서 전략 객체를 보유하고, 클라이언트의 요청을 전략에 위임하는 역할을 합니다. Context 자신은 구체적인 알고리즘을 알지 못하고, 전략 인터페이스에만 의존합니다.

### 상세 설명

**Context의 책임:**
1. 전략 객체 참조 보유
2. 필요한 데이터를 전략에 전달
3. 전략 교체 인터페이스 제공
4. 공통 로직 처리 (전략 호출 전후)

```javascript
class PaymentContext {
    #strategy; // 전략 객체 참조

    constructor(strategy) {
        this.#strategy = strategy;
    }

    // 전략 교체 인터페이스
    setStrategy(strategy) {
        this.#strategy = strategy;
    }

    // 전략에 위임
    processPayment(amount) {
        // 공통 로직 (전처리)
        console.log(`결제 시작: ${amount}원`);
        this.validateAmount(amount);

        // 전략에 위임 (구체 알고리즘 모름)
        const result = this.#strategy.pay(amount);

        // 공통 로직 (후처리)
        console.log('결제 완료');
        return result;
    }

    validateAmount(amount) {
        if (amount <= 0) throw new Error('유효하지 않은 금액');
    }
}

// 클라이언트
const ctx = new PaymentContext(new KakaoPayment());
ctx.processPayment(10000);

ctx.setStrategy(new CardPayment('1234-5678'));
ctx.processPayment(20000);
```

**Context와 Strategy의 데이터 공유 방법:**
1. 메서드 인수로 전달 (권장): `strategy.pay(amount)`
2. Context 자신을 전달: `strategy.execute(this)`

### 꼬리 질문
- Context가 Strategy에 자기 자신을 전달하는 방식의 장단점은?
- Context 없이 전략 패턴을 구현할 수 있나요?

### 실무 사례
Spring의 `JdbcTemplate`이 Context 역할을 합니다. SQL 실행 전략(`RowMapper`)을 받아 공통 연결/예외처리 로직을 담당하고 실제 결과 매핑만 전략에 위임합니다.
