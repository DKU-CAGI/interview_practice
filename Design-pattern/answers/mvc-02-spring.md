## Q. 스프링(Spring)에서 MVC 패턴이 어떻게 구현되는지 설명해주세요.

### 핵심 답변
Spring MVC는 `DispatcherServlet`을 프론트 컨트롤러로 두어 모든 HTTP 요청을 받고, `@Controller`, `@Service`, `@Repository`, JPA/MyBatis 등으로 MVC를 구현합니다.

### 상세 설명

**Spring MVC 요청 처리 흐름:**
```
요청 → DispatcherServlet → HandlerMapping → @Controller
     ← ModelAndView ←  @Controller ↔ @Service ↔ @Repository ↔ DB
DispatcherServlet → ViewResolver → View(JSP/Thymeleaf) → 응답
```

**각 구성요소:**
```java
// Controller: 요청 수신 및 응답 반환
@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        UserDto user = userService.findById(id);
        return ResponseEntity.ok(user);
    }

    @PostMapping
    public ResponseEntity<UserDto> createUser(@RequestBody @Valid UserDto dto) {
        UserDto created = userService.create(dto);
        return ResponseEntity.status(201).body(created);
    }
}

// Service: 비즈니스 로직 (Model의 일부)
@Service
@Transactional
public class UserService {
    private final UserRepository userRepository;

    public UserDto findById(Long id) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new NotFoundException("사용자 없음"));
        return UserDto.from(user);
    }
}

// Repository: 데이터 접근 (Model의 일부)
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}

// Model (Entity)
@Entity
@Table(name = "users")
public class User {
    @Id @GeneratedValue
    private Long id;
    private String name;
    private String email;
}
```

**View:**
- REST API: `@ResponseBody`로 JSON 반환 (View 없음)
- 서버 사이드 렌더링: Thymeleaf, JSP

### 꼬리 질문
- DispatcherServlet이 프론트 컨트롤러 패턴인 이유는?
- `@RestController`와 `@Controller`의 차이는?

### 실무 사례
현대 Spring Boot 프로젝트는 REST API + React/Vue 프론트엔드 조합이 일반적입니다. View 역할은 프론트엔드 프레임워크가 담당합니다.
