## Q. 메인 모듈과 하위 모듈 간의 의존성을 어떻게 관리하나요?

### 핵심 답변
메인 모듈(앱 진입점)이 의존성 그래프를 구성하고 하위 모듈에 주입하는 방식으로 관리합니다. 이를 "Composition Root" 패턴이라고 하며, 의존성 연결을 한 곳에 모아 관리합니다.

### 상세 설명

**핵심 원칙: Composition Root**
```
앱 진입점(main 함수, ApplicationContext) 에서만 구체 클래스를 참조
나머지 코드는 인터페이스에만 의존
```

**계층 구조와 의존성 방향:**
```
[Presentation Layer] (Controller)
        ↓ (의존)
[Application Layer] (Service, UseCase)
        ↓ (의존)
[Domain Layer] (Entity, Repository Interface)
        ↓ (의존)
[Infrastructure Layer] (Repository Impl, DB, External API)
```

**Node.js 예시 (Composition Root):**
```javascript
// infrastructure/database.js
const db = new Database(process.env.DB_URL);

// infrastructure/repositories/user.repo.js
const userRepo = new UserRepository(db);

// application/services/user.service.js
const emailSender = new EmailSender(process.env.SMTP_HOST);
const userService = new UserService(userRepo, emailSender);

// presentation/controllers/user.controller.js
const userController = new UserController(userService);

// main.js (Composition Root) — 여기서만 의존성 연결
const app = express();
app.use('/users', userController.router);
app.listen(3000);
```

**의존성 역방향 흐름 방지:**
```javascript
// ❌ 하위 모듈이 상위를 알면 안 됨
class UserRepository {
    constructor() {
        this.userService = new UserService(this); // 순환 의존성!
    }
}

// ✅ 단방향 의존성
class UserService {
    constructor(userRepository) { // 상위가 하위를 주입받음
        this.userRepository = userRepository;
    }
}
```

**순환 의존성 해결:**
- 공통 의존성을 별도 모듈로 추출
- 이벤트 발행/구독으로 의존성 역방향 제거

### 꼬리 질문
- 순환 의존성이 발생했을 때 어떻게 진단하고 해결하나요?
- Clean Architecture에서 의존성 방향 규칙을 설명해주세요.

### 실무 사례
NestJS는 `@Module` 데코레이터로 모듈 단위 의존성을 선언하고, 컨테이너가 자동으로 Composition Root를 구성합니다.
