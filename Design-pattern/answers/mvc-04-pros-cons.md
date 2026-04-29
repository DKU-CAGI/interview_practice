## Q. MVC 패턴의 장점과 단점은 무엇인가요?

### 핵심 답변
MVC는 관심사 분리로 유지보수성과 테스트 용이성을 높이지만, Controller 비대화와 View-Model 간 강한 의존성이 단점입니다.

### 상세 설명

**장점:**

**1. 관심사 분리 (Separation of Concerns)**
- 데이터 로직(Model), UI(View), 흐름 제어(Controller)가 독립적
- 한 부분의 변경이 다른 부분에 미치는 영향 최소화

**2. 병렬 개발 가능**
- 프론트엔드(View)와 백엔드(Model/Controller)가 독립적으로 개발 가능

**3. 테스트 용이성**
- Model을 View 없이 단위 테스트 가능
- Controller도 Mock View/Model로 테스트 가능

**4. 재사용성**
- 동일한 Model을 다른 View로 표시 가능 (모바일/웹)

**단점:**

**1. Massive ViewController (Controller 비대화)**
```java
// 현실에서 Controller가 너무 많은 일을 함
@PostMapping("/checkout")
public ResponseEntity<Order> checkout(@RequestBody CheckoutDto dto) {
    // 입력 검증
    // 재고 확인
    // 결제 처리
    // 주문 생성
    // 이메일 발송
    // 포인트 적립
    // ... 수백 줄
}
```

**2. View-Model 의존성**
- 전통적 MVC에서 View가 Model을 직접 참조 → 결합도 증가

**3. 소규모 프로젝트엔 오버헤드**
- 간단한 CRUD에도 최소 3개 파일/클래스 필요

**4. 복잡한 View 상태 관리**
- 양방향 데이터 흐름이 필요한 복잡한 UI에서 한계
- → MVVM 패턴 등장의 배경

### 꼬리 질문
- Controller 비대화 문제를 해결하기 위한 방법은 무엇인가요?
- MVC의 단점이 MVP, MVVM 패턴 등장에 어떻게 영향을 미쳤나요?

### 실무 사례
Spring에서는 Controller를 얇게(thin) 유지하고 비즈니스 로직을 Service 레이어로 분리하는 것이 권장 패턴입니다.
