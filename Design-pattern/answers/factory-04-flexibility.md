## Q. 팩토리 패턴을 사용하면 코드의 유연성이 어떻게 향상되나요?

### 핵심 답변
팩토리 패턴은 객체 생성 로직을 한 곳에 캡슐화하여, 새로운 타입 추가나 생성 방식 변경 시 클라이언트 코드를 수정하지 않아도 됩니다.

### 상세 설명

**팩토리 없을 때의 문제:**
```javascript
// 클라이언트 코드에 생성 로직 분산
function processPayment(type) {
    let payment;
    if (type === 'card') payment = new CardPayment();
    else if (type === 'kakao') payment = new KakaoPayment();
    else if (type === 'naver') payment = new NaverPayment(); // 추가할 때마다 수정
    payment.process();
}
```

**팩토리 패턴 적용 후:**
```javascript
class PaymentFactory {
    static create(type) {
        const strategies = {
            card: CardPayment,
            kakao: KakaoPayment,
            naver: NaverPayment,
            // 새 결제 추가: 여기만 수정
        };
        const PaymentClass = strategies[type];
        if (!PaymentClass) throw new Error(`Unknown payment: ${type}`);
        return new PaymentClass();
    }
}

function processPayment(type) {
    const payment = PaymentFactory.create(type); // 클라이언트 코드 불변
    payment.process();
}
```

**유연성이 향상되는 이유:**
1. **OCP 준수**: 새 타입 추가 시 팩토리만 수정, 클라이언트 코드 불변
2. **의존성 역전**: 클라이언트는 인터페이스에만 의존, 구체 클래스와 분리
3. **테스트 용이**: 팩토리를 Mock으로 교체하면 임의 객체 반환 가능
4. **런타임 결정**: 어떤 클래스를 생성할지 런타임에 결정 가능

### 꼬리 질문
- 팩토리 패턴이 OCP(개방-폐쇄 원칙)를 어떻게 준수하나요?
- 팩토리 패턴과 전략 패턴을 함께 사용하는 경우를 설명해주세요.

### 실무 사례
결제 시스템에서 PG사별 결제 모듈, 알림 시스템에서 채널별(이메일/SMS/푸시) 발송 객체를 팩토리로 생성합니다.
