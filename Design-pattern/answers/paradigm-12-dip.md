## Q. 의존 역전 원칙(DIP)이란 무엇인가요?

### 핵심 답변
DIP는 "고수준 모듈이 저수준 모듈에 직접 의존하지 말고, 둘 다 추상(인터페이스)에 의존해야 한다"는 원칙입니다. 구체 클래스가 아닌 인터페이스에 의존하면 구현체를 자유롭게 교체할 수 있습니다.

### 상세 설명

**의존성의 방향:**
```
❌ 전통적 의존성: 고수준 → 저수준 (구체 클래스)
✅ 역전된 의존성: 고수준 → 추상 ← 저수준
```

**DIP 위반:**
```java
// 고수준 모듈(OrderService)이 저수준 모듈(MySQLDB)에 직접 의존
class OrderService {
    private MySQLDatabase db = new MySQLDatabase(); // 구체 클래스 의존

    void createOrder(Order order) {
        db.save(order); // MySQLDatabase에 종속
    }
}
// MySQL을 MongoDB로 바꾸려면 OrderService 수정 필요
```

**DIP 준수:**
```java
// 추상화 정의
interface OrderRepository {
    void save(Order order);
    Order findById(Long id);
}

// 저수준 모듈이 추상화를 구현
class MySQLOrderRepository implements OrderRepository {
    void save(Order order) { /* MySQL 저장 */ }
    Order findById(Long id) { /* MySQL 조회 */ }
}

class MongoOrderRepository implements OrderRepository {
    void save(Order order) { /* MongoDB 저장 */ }
    Order findById(Long id) { /* MongoDB 조회 */ }
}

// 고수준 모듈이 추상화에 의존
class OrderService {
    private final OrderRepository repository; // 인터페이스에 의존

    OrderService(OrderRepository repository) { // DI로 주입
        this.repository = repository;
    }

    void createOrder(Order order) {
        repository.save(order); // 어떤 구현체든 동작
    }
}

// 구현체 교체: OrderService 수정 없음
new OrderService(new MySQLOrderRepository());
new OrderService(new MongoOrderRepository()); // 테스트용
```

**DIP vs DI:**
- DIP: "추상에 의존하라" — **설계 원칙**
- DI: DIP를 실현하기 위한 **구현 기법**

### 꼬리 질문
- DIP를 적용하지 않으면 어떤 문제가 발생하나요?
- 인터페이스 없이 DIP를 달성할 수 있나요?

### 실무 사례
Spring의 `@Repository` 인터페이스 + JPA 구현체 패턴이 DIP의 교과서적 적용입니다. 테스트 시 인메모리 구현체로 교체하여 빠른 단위 테스트가 가능합니다.
