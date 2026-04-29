## Q. 의존성 역전 원칙과 의존성 주입의 관계를 설명해주세요.

### 핵심 답변
의존성 역전 원칙(DIP)은 "추상에 의존하고 구체에 의존하지 마라"는 설계 원칙이고, 의존성 주입(DI)은 DIP를 실현하는 구현 기법입니다. DIP가 목표이고 DI가 수단입니다.

### 상세 설명

**의존성 역전 원칙 (DIP):**
- 고수준 모듈이 저수준 모듈에 직접 의존하면 안 됨
- 둘 다 추상(인터페이스)에 의존해야 함

**DIP 위반:**
```java
// 고수준 모듈 (UserService)이 저수준 모듈 (MySQLDatabase)에 직접 의존
class UserService {
    private MySQLDatabase db; // 구체 클래스에 의존 → DIP 위반

    public UserService() {
        this.db = new MySQLDatabase(); // 강한 결합
    }
}
```

**DIP 준수 + DI 적용:**
```java
// 추상화 (인터페이스 정의)
public interface UserRepository {
    User findById(Long id);
    User save(User user);
}

// 저수준 모듈 (추상에 의존)
public class MySQLUserRepository implements UserRepository {
    @Override
    public User findById(Long id) { /* MySQL 쿼리 */ }
}

public class MongoUserRepository implements UserRepository {
    @Override
    public User findById(Long id) { /* MongoDB 쿼리 */ }
}

// 고수준 모듈 (추상에 의존 + DI로 주입받음)
public class UserService {
    private final UserRepository userRepository; // 인터페이스에 의존

    public UserService(UserRepository userRepository) { // DI
        this.userRepository = userRepository;
    }

    public User getUser(Long id) {
        return userRepository.findById(id);
    }
}

// 조립 (DI 컨테이너 역할)
UserRepository repo = new MySQLUserRepository();
UserService service = new UserService(repo); // 주입
```

**의존성 방향의 역전:**
```
DIP 이전: UserService → MySQLDatabase (고수준이 저수준에 의존)
DIP 이후: UserService → UserRepository ← MySQLDatabase
                        (둘 다 추상에 의존)
```

### 꼬리 질문
- DIP와 OCP의 관계는 무엇인가요?
- 인터페이스 없이 DIP를 지킬 수 있나요?

### 실무 사례
Spring에서 `@Repository` 인터페이스를 선언하고 JPA 구현체를 주입받는 것이 DIP + DI의 대표적 예입니다. 테스트 시 인메모리 구현체로 교체합니다.
