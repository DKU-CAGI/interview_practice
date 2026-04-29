## Q. 단일 책임 원칙(SRP)이란 무엇인가요?

### 핵심 답변
단일 책임 원칙(SRP)은 "하나의 클래스는 변경의 이유가 하나뿐이어야 한다"는 원칙입니다. 클래스가 하나의 기능(책임)만 담당해야 변경이 다른 기능에 영향을 주지 않습니다.

### 상세 설명

**"책임"의 의미:**
SRP에서 책임은 "변경의 이유"를 의미합니다. 클래스를 수정해야 할 이유가 여러 개라면 SRP를 위반한 것입니다.

**SRP 위반:**
```javascript
class UserService {
    // 책임 1: 사용자 데이터 관리
    async createUser(userData) { /* ... */ }
    async getUserById(id) { /* ... */ }
    async updateUser(id, data) { /* ... */ }

    // 책임 2: 이메일 발송 (다른 이유로 변경될 수 있음)
    sendWelcomeEmail(user) { /* ... */ }
    sendPasswordResetEmail(user) { /* ... */ }

    // 책임 3: 보고서 생성 (또 다른 변경 이유)
    generateUserReport() { /* ... */ }
}
// 이메일 서버가 바뀌면 UserService 수정 필요 → 위반
```

**SRP 준수:**
```javascript
// 각 클래스는 하나의 변경 이유만 가짐
class UserRepository {
    async create(userData) { /* DB 저장 */ }
    async findById(id) { /* DB 조회 */ }
}

class EmailService {
    sendWelcomeEmail(user) { /* SMTP 발송 */ }
    sendPasswordResetEmail(user) { /* ... */ }
}

class UserReportGenerator {
    generate(users) { /* 보고서 생성 */ }
}

class UserApplicationService {
    constructor(userRepo, emailService) {
        this.userRepo = userRepo;
        this.emailService = emailService;
    }
    async createUser(userData) {
        const user = await this.userRepo.create(userData);
        this.emailService.sendWelcomeEmail(user);
        return user;
    }
}
```

**SRP의 이점:**
- 변경 범위 최소화
- 클래스 재사용성 향상
- 테스트 용이 (각 클래스를 독립 테스트)
- 코드 이해 용이

**SRP 판단 기준:**
"이 클래스를 수정해야 하는 이유가 몇 가지인가?"

### 꼬리 질문
- SRP와 응집도(Cohesion)의 관계는?
- 너무 작은 클래스로 분리하면 어떤 문제가 생기나요?

### 실무 사례
Spring의 3계층(Controller, Service, Repository)이 SRP를 적용한 대표 예입니다. Controller는 HTTP 처리, Service는 비즈니스 로직, Repository는 데이터 접근만 담당합니다.
