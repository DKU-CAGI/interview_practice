## Q. 개방-폐쇄 원칙(OCP)이란 무엇인가요?

### 핵심 답변
OCP는 "소프트웨어 요소는 확장에는 열려있고, 수정에는 닫혀있어야 한다"는 원칙입니다. 새 기능 추가 시 기존 코드를 수정하지 않고 새 코드를 추가하는 방식으로 확장해야 합니다.

### 상세 설명

**OCP 위반:**
```javascript
// 새 결제 수단 추가마다 processPayment 수정 필요 → OCP 위반
function processPayment(type, amount) {
    if (type === 'card') { /* 카드 결제 */ }
    else if (type === 'kakao') { /* 카카오페이 */ }
    else if (type === 'naver') { /* 네이버페이 → 추가마다 이 함수 수정 */ }
}
```

**OCP 준수 (전략 패턴 활용):**
```javascript
// 기존 코드 수정 없이 새 결제 수단 추가 가능
class PaymentProcessor {
    constructor(strategy) { this.strategy = strategy; }
    process(amount) { return this.strategy.pay(amount); }
}

class CardPayment {
    pay(amount) { console.log(`카드 ${amount}원`); }
}
class KakaoPayment {
    pay(amount) { console.log(`카카오 ${amount}원`); }
}
// 새 결제 수단: 기존 코드 수정 없이 추가
class NaverPayment {
    pay(amount) { console.log(`네이버 ${amount}원`); }
}

// 사용
const processor = new PaymentProcessor(new NaverPayment());
processor.process(10000);
```

**OCP를 달성하는 방법:**
1. **추상화/인터페이스**: 구현이 아닌 추상에 의존
2. **전략 패턴**: 알고리즘을 교체 가능하게
3. **데코레이터 패턴**: 기능 추가 시 기존 클래스 수정 없이
4. **플러그인 아키텍처**: 새 기능을 플러그인으로 추가

**OCP와 다른 원칙의 관계:**
- DIP(의존 역전)와 밀접: 추상에 의존해야 OCP 달성 가능
- 전략/팩토리 패턴으로 OCP 구현

### 꼬리 질문
- OCP를 100% 준수하는 것이 가능한가요?
- OCP 위반을 리팩토링할 때 어떤 패턴을 주로 사용하나요?

### 실무 사례
Spring의 `HandlerInterceptor`를 구현하면 기존 컨트롤러 수정 없이 요청 전처리를 추가할 수 있습니다. 이것이 OCP의 실용적 적용입니다.
