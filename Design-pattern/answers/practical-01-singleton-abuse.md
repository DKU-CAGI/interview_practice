## Q. 싱글톤 패턴을 남용하면 어떤 문제가 발생하나요?

### 핵심 답변
싱글톤 남용은 전역 상태 오염, 강한 결합, 테스트 불가능, 멀티스레드 위험, 단일 장애점(SPOF) 등 설계와 유지보수를 심각하게 저하시킵니다.

### 상세 설명

**1. 전역 상태 오염 (Shared Mutable State)**
```javascript
// 어플리케이션 어디서든 변경 가능 → 예측 불가
const config = ConfigManager.getInstance();
config.setDebugMode(true); // 어디서 변경됐는지 추적 불가
```

**2. 강한 결합 (Tight Coupling)**
```javascript
// 모든 코드가 GlobalCache에 의존 → 테스트/변경 불가
class UserService {
    getUser(id) {
        return GlobalCache.getInstance().get(`user:${id}`); // 싱글톤 직접 참조
    }
}
```

**3. 테스트 격리 불가**
```javascript
// 테스트 A에서 변경한 싱글톤 상태가 테스트 B에 영향
test('A: 로그인 성공', () => {
    SessionManager.getInstance().setUser(mockUser);
    // ...
});

test('B: 비로그인 상태 테스트', () => {
    // SessionManager에 A의 mockUser가 남아있음!
    SessionManager.getInstance().getUser(); // 예상과 다른 결과
});
```

**4. 멀티스레드 위험 (Java)**
```java
// 동기화 없는 싱글톤 = Race Condition
if (instance == null) {           // 스레드 A, B 동시에 통과
    instance = new Singleton();   // 두 개 생성 가능!
}
```

**5. SOLID 원칙 위반**
- SRP: 싱글톤 관리 + 비즈니스 로직을 동시에
- OCP: 구현 교체 불가
- DIP: 구체 클래스에 직접 의존

**적절한 사용 기준:**
- 진짜로 하나여야 하는가? (DB 연결 풀, 설정 등)
- DI 컨테이너로 관리 가능한가?
- 테스트 시 Mock으로 교체 가능한가?

### 꼬리 질문
- 싱글톤을 DI 컨테이너로 대체하면 어떤 이점이 있나요?
- Monostate 패턴이 싱글톤의 대안이 되는 이유는?

### 실무 사례
Node.js에서 싱글톤 대신 모듈 캐싱 + 의존성 주입 패턴을 사용하는 것이 권장됩니다. Spring은 IoC 컨테이너가 싱글톤 관리를 담당하여 직접 싱글톤 구현을 피합니다.
