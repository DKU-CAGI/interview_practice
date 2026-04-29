## Q. 여러 디자인 패턴을 조합해서 사용할 수 있나요? 예를 들어주세요.

### 핵심 답변
네, 실무에서는 여러 패턴을 조합하여 사용하는 것이 일반적입니다. 하나의 패턴이 다른 패턴을 필요로 하거나, 서로 보완적인 관계를 가집니다.

### 상세 설명

**예시 1: 팩토리 + 전략 패턴**
```javascript
// 결제 수단을 팩토리로 생성하고 전략으로 실행
class PaymentFactory {
    static create(type) {
        const strategies = {
            card: CardStrategy,
            kakao: KakaoStrategy,
            naver: NaverStrategy,
        };
        return new strategies[type]();
    }
}

class CheckoutService {
    processPayment(type, amount) {
        const strategy = PaymentFactory.create(type); // 팩토리
        strategy.pay(amount); // 전략
    }
}
```

**예시 2: 싱글톤 + 옵저버 패턴 (이벤트 버스)**
```javascript
class EventBus {
    static #instance = null; // 싱글톤
    #listeners = new Map(); // 옵저버 목록

    static getInstance() {
        if (!EventBus.#instance) {
            EventBus.#instance = new EventBus();
        }
        return EventBus.#instance;
    }

    on(event, handler) { // 옵저버 등록
        if (!this.#listeners.has(event)) {
            this.#listeners.set(event, []);
        }
        this.#listeners.get(event).push(handler);
    }

    emit(event, data) { // 옵저버 알림
        this.#listeners.get(event)?.forEach(h => h(data));
    }
}
```

**예시 3: 데코레이터 + 프록시 + 싱글톤 (Spring AOP)**
```java
@Service // 싱글톤
@Transactional // 프록시 + 데코레이터 (트랜잭션 래핑)
@Cacheable("users") // 프록시 (캐싱)
public class UserService {
    public User findById(Long id) { /* 비즈니스 로직만 */ }
}
```

**예시 4: MVC + 옵저버 패턴**
```
Model (Subject) → Observer 알림 → View (Observer) 자동 갱신
Controller는 Model을 변경하는 역할
```

**패턴 조합 시 주의사항:**
- 불필요한 패턴 추가 = 복잡성 증가
- 각 패턴의 역할과 책임이 명확해야 함

### 꼬리 질문
- 패턴을 조합할 때 결합도가 높아지는 문제를 어떻게 방지하나요?
- MVC + 옵저버 패턴의 조합이 React에서 어떻게 구현되나요?

### 실무 사례
Redux는 옵저버(store.subscribe) + Flux 패턴 + 불변 상태 관리를 조합합니다. NestJS는 DI + 데코레이터 + 모듈 패턴을 조합합니다.
