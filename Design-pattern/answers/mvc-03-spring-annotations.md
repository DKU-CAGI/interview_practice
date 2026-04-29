## Q. @RequestParam, @RequestHeader, @PathVariable의 차이점은?

### 핵심 답변
`@PathVariable`은 URL 경로의 일부에서, `@RequestParam`은 쿼리 파라미터에서, `@RequestHeader`는 HTTP 헤더에서 값을 추출합니다.

### 상세 설명

**@PathVariable — URL 경로 변수**
```java
// URL: GET /users/42
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}

// 여러 경로 변수
// URL: GET /users/42/orders/7
@GetMapping("/users/{userId}/orders/{orderId}")
public Order getOrder(
    @PathVariable Long userId,
    @PathVariable Long orderId
) { ... }
```

**@RequestParam — 쿼리 파라미터 또는 폼 데이터**
```java
// URL: GET /users?page=1&size=10&keyword=홍
@GetMapping("/users")
public List<User> getUsers(
    @RequestParam(defaultValue = "1") int page,
    @RequestParam(defaultValue = "10") int size,
    @RequestParam(required = false) String keyword // 선택적
) {
    return userService.search(page, size, keyword);
}
```

**@RequestHeader — HTTP 헤더**
```java
// 헤더: Authorization: Bearer eyJhb...
@GetMapping("/profile")
public UserDto getProfile(
    @RequestHeader("Authorization") String authHeader,
    @RequestHeader(value = "X-Request-ID", required = false) String requestId
) {
    String token = authHeader.replace("Bearer ", "");
    return authService.getProfileByToken(token);
}
```

**비교:**
| 어노테이션 | 위치 | 예시 URL |
|-----------|------|---------|
| `@PathVariable` | URL 경로 | `/users/{42}` |
| `@RequestParam` | URL 쿼리스트링 | `/users?id=42` |
| `@RequestHeader` | HTTP 헤더 | `Authorization: Bearer ...` |

**추가: @RequestBody**
```java
// POST 요청의 JSON 바디 전체를 객체로 변환
@PostMapping("/users")
public User create(@RequestBody @Valid UserDto dto) { ... }
```

### 꼬리 질문
- `@RequestParam`과 `@RequestBody`의 차이는 무엇인가요?
- `@PathVariable`에서 타입 변환(Long, UUID 등)은 어떻게 처리되나요?

### 실무 사례
REST API 설계 원칙: 리소스 식별에는 `@PathVariable`, 필터링/페이지네이션엔 `@RequestParam`, 인증 토큰엔 `@RequestHeader`를 사용합니다.
