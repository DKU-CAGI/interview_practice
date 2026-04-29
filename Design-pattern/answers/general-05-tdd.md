## Q. TDD(Test Driven Development)와 디자인 패턴의 관계는?

### 핵심 답변
TDD는 테스트 가능한 코드를 작성하도록 강제하고, 이 과정에서 의존성 주입, 단일 책임 원칙 등 좋은 설계 원칙이 자연스럽게 적용됩니다. TDD를 따르면 패턴 적용이 유기적으로 이루어집니다.

### 상세 설명

**TDD의 사이클:**
```
Red → Green → Refactor
(실패 테스트 작성 → 최소 구현 → 리팩토링)
```

**TDD가 디자인 패턴을 유도하는 과정:**

**1. DI(의존성 주입)로 유도**
```javascript
// 테스트를 먼저 작성하면 Mock 주입 필요 → 자연스럽게 DI 적용
test('사용자 생성 시 이메일 발송', () => {
    const mockEmailSender = { send: jest.fn() };
    // DI 없이는 Mock 주입 불가 → DI 설계를 강제
    const service = new UserService(mockEmailSender);
    service.createUser({ email: 'test@test.com' });
    expect(mockEmailSender.send).toHaveBeenCalled();
});
```

**2. 단일 책임 원칙(SRP) 유도**
```javascript
// UserService에서 이메일 전송이 어렵게 느껴지면
// → 이메일 발송을 별도 클래스로 분리 (SRP)
// → EmailService 추출 → DI로 주입
```

**3. 전략 패턴 유도**
```javascript
// 결제 방법마다 테스트 케이스가 늘어날 때
// → 각 결제 방법을 전략으로 분리 → 독립 테스트 가능
test('카드 결제', () => { /* ... */ });
test('카카오페이 결제', () => { /* ... */ });
```

**TDD로 자연스럽게 준수되는 원칙:**
- SRP: 테스트하기 어려우면 클래스 분리
- OCP: 새 테스트 추가 시 기존 코드 수정 최소화
- DIP: Mock 주입을 위해 인터페이스에 의존

**TDD의 한계:**
- UI 테스트는 어려움 (E2E 테스트 필요)
- 초기 개발 속도 감소
- 과도한 Mock 사용 → 실제 통합 테스트 부재

### 꼬리 질문
- BDD(Behavior Driven Development)와 TDD의 차이는?
- 레거시 코드에 TDD를 적용할 때 어려움은?

### 실무 사례
테스트 커버리지가 높은 코드베이스는 대부분 좋은 설계 패턴을 자연스럽게 포함합니다. TDD는 설계 품질 향상의 부산물로 좋은 패턴을 적용하게 합니다.
