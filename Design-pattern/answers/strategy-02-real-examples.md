## Q. 전략 패턴을 사용하는 실제 사례를 들어주세요 (예: 결제 시스템).

### 핵심 답변
결제 시스템에서 신용카드, 카카오페이, 네이버페이 등 결제 수단을 전략으로 캡슐화하면, 새 결제 수단 추가 시 기존 코드를 변경하지 않고 전략만 추가하면 됩니다.

### 상세 설명

**결제 시스템 예시:**
```javascript
// Strategy 인터페이스
class PaymentStrategy {
    pay(amount) { throw new Error('구현 필요'); }
}

// ConcreteStrategy
class CardPayment extends PaymentStrategy {
    constructor(cardNumber) {
        super();
        this.cardNumber = cardNumber;
    }
    pay(amount) {
        console.log(`카드 ${this.cardNumber}로 ${amount}원 결제`);
    }
}

class KakaoPayment extends PaymentStrategy {
    pay(amount) {
        console.log(`카카오페이로 ${amount}원 결제`);
    }
}

class NaverPayment extends PaymentStrategy {
    pay(amount) {
        console.log(`네이버페이로 ${amount}원 결제`);
    }
}

// Context
class ShoppingCart {
    constructor(paymentStrategy) {
        this.strategy = paymentStrategy;
        this.items = [];
    }

    addItem(item) { this.items.push(item); }

    checkout() {
        const total = this.items.reduce((sum, item) => sum + item.price, 0);
        this.strategy.pay(total);
    }
}

// 사용
const cart = new ShoppingCart(new KakaoPayment());
cart.addItem({ name: '노트북', price: 1500000 });
cart.checkout(); // 카카오페이로 1500000원 결제
```

**다른 실제 사례:**
- **정렬 알고리즘**: 데이터 크기에 따라 퀵소트/머지소트 동적 선택
- **압축**: 파일 형식에 따라 zip/gzip/bzip2 선택
- **인증**: 로컬/OAuth/JWT 등 인증 방식 교체
- **로깅**: 콘솔/파일/데이터베이스 로거 교체

### 꼬리 질문
- 결제 전략을 런타임에 변경해야 하는 상황을 설명해주세요.
- 전략 패턴과 if-else 분기문의 차이는 무엇인가요?

### 실무 사례
PG사 연동 시스템에서 각 PG사(KG이니시스, 토스페이먼츠, 나이스페이)를 별도 전략 클래스로 구현하여 설정값에 따라 런타임에 결정합니다.
