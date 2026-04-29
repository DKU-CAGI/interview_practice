## Q. 의존성 주입이란 무엇이며, 왜 필요한가요?

### 핵심 답변
의존성 주입(DI)은 객체가 스스로 의존하는 객체를 생성하는 대신, 외부에서 주입받는 설계 기법입니다. 결합도를 낮추고, 테스트 용이성과 유연성을 높이기 위해 사용합니다.

### 상세 설명

**DI 없을 때의 문제:**
```javascript
// UserService가 EmailSender를 직접 생성 → 강한 결합
class UserService {
    constructor() {
        this.emailSender = new GmailSender(); // 하드코딩된 의존성
    }

    createUser(userData) {
        const user = db.save(userData);
        this.emailSender.send(user.email, '환영합니다!'); // GmailSender에 종속
        return user;
    }
}
// GmailSender를 NaverSender로 바꾸려면 UserService 코드 수정 필요
// 테스트 시 실제 이메일이 발송됨
```

**DI 적용 후:**
```javascript
class UserService {
    constructor(emailSender) { // 외부에서 주입
        this.emailSender = emailSender;
    }

    createUser(userData) {
        const user = db.save(userData);
        this.emailSender.send(user.email, '환영합니다!');
        return user;
    }
}

// 프로덕션: Gmail 사용
const service = new UserService(new GmailSender());

// 테스트: Mock 사용 (실제 이메일 발송 없음)
const mockSender = { send: jest.fn() };
const testService = new UserService(mockSender);
```

**DI의 세 가지 방식:**
1. **생성자 주입** (가장 권장): `constructor(dep) { this.dep = dep; }`
2. **Setter 주입**: `setDep(dep) { this.dep = dep; }`
3. **인터페이스 주입**: 인터페이스로 주입 메서드 강제

**DI가 필요한 이유:**
- 결합도 감소 (구체 클래스 대신 인터페이스 의존)
- 단위 테스트 용이 (Mock 주입)
- 런타임 전략 변경 가능
- 재사용성 향상

### 꼬리 질문
- DI와 의존성 역전 원칙(DIP)의 관계는 무엇인가요?
- DI 컨테이너를 사용하면 어떤 이점이 있나요?

### 실무 사례
Spring의 `@Autowired`, `@Inject`, NestJS의 `@Injectable`, Angular의 DI 시스템이 모두 의존성 주입 컨테이너입니다.
