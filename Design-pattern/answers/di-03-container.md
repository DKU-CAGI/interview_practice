## Q. 의존성 주입 컨테이너(Dependency Injector)의 역할은?

### 핵심 답변
DI 컨테이너는 객체의 생성과 의존성 연결을 자동화하는 프레임워크입니다. 개발자가 직접 `new`를 사용해 객체를 생성하고 주입하는 과정을 대신 처리합니다.

### 상세 설명

**DI 컨테이너 없을 때:**
```javascript
// 수동으로 의존성 연결 — 복잡해질수록 유지보수 어려움
const db = new DatabaseConnection('localhost', 3306);
const userRepo = new UserRepository(db);
const emailSender = new EmailSender('smtp.gmail.com');
const notificationService = new NotificationService(emailSender);
const userService = new UserService(userRepo, notificationService);
const userController = new UserController(userService);
```

**DI 컨테이너 사용:**
```javascript
// 컨테이너가 의존성 그래프를 자동으로 분석하고 연결
container.register(DatabaseConnection);
container.register(UserRepository);
container.register(EmailSender);
container.register(NotificationService);
container.register(UserService);

// 루트 객체만 요청하면 전체 그래프 자동 생성
const userController = container.resolve(UserController);
```

**Spring의 DI 컨테이너:**
```java
@Service // "이 클래스를 컨테이너에 등록"
public class UserService {
    private final UserRepository repository;
    private final NotificationService notificationService;

    // 생성자 주입: 컨테이너가 자동으로 UserRepository, NotificationService 찾아 주입
    public UserService(UserRepository repository,
                       NotificationService notificationService) {
        this.repository = repository;
        this.notificationService = notificationService;
    }
}

// 사용
@RestController
public class UserController {
    @Autowired // 컨테이너가 UserService 찾아 주입
    private UserService userService;
}
```

**컨테이너의 주요 기능:**
1. **빈 등록**: 객체 생성 방법 등록
2. **의존성 해결**: 의존성 그래프 분석 및 자동 주입
3. **스코프 관리**: 싱글톤/프로토타입/요청 스코프
4. **생명주기 관리**: `@PostConstruct`, `@PreDestroy`

### 꼬리 질문
- DI 컨테이너와 서비스 로케이터(Service Locator) 패턴의 차이는?
- Spring의 ApplicationContext와 BeanFactory의 차이는?

### 실무 사례
Spring ApplicationContext, NestJS IoC Container, Angular DI, Inversify.js(TypeScript)가 DI 컨테이너의 예입니다.
