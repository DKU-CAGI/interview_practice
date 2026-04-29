## Q. SOLID 원칙에 대해 설명해주세요.

### 핵심 답변
SOLID는 Robert C. Martin이 제시한 OOP 설계의 5가지 원칙입니다: 단일 책임(SRP), 개방-폐쇄(OCP), 리스코프 치환(LSP), 인터페이스 분리(ISP), 의존 역전(DIP).

### 상세 설명

**S — Single Responsibility Principle (단일 책임 원칙)**
> 클래스는 하나의 책임만 가져야 한다.
```java
// ❌ 여러 책임
class User {
    void saveToDatabase() { /* DB 저장 */ }
    void sendEmail() { /* 이메일 발송 */ }
    void generateReport() { /* 보고서 생성 */ }
}

// ✅ 책임 분리
class User { /* 사용자 데이터만 */ }
class UserRepository { void save(User u) { /* DB 저장 */ } }
class EmailService { void send(User u) { /* 이메일 */ } }
```

**O — Open/Closed Principle (개방-폐쇄 원칙)**
> 확장에는 열려있고, 수정에는 닫혀있어야 한다.
```java
// ✅ 새 도형 추가 시 기존 코드 수정 없이 확장
interface Shape { double area(); }
class Circle implements Shape { double area() { return Math.PI * r * r; } }
class Square implements Shape { double area() { return side * side; } }
```

**L — Liskov Substitution Principle (리스코프 치환 원칙)**
> 자식 클래스는 부모 클래스를 완전히 대체할 수 있어야 한다.
```java
// ❌ Square는 Rectangle을 대체할 수 없음 (setWidth가 높이도 변경)
class Square extends Rectangle { /* 위반 */ }

// ✅ 별도 계층 구조
interface Shape { double area(); }
```

**I — Interface Segregation Principle (인터페이스 분리 원칙)**
> 클라이언트가 사용하지 않는 메서드에 의존해서는 안 된다.
```java
// ❌ 모든 구현체가 불필요한 메서드를 구현해야 함
interface Worker { void work(); void eat(); void sleep(); }

// ✅ 인터페이스 분리
interface Workable { void work(); }
interface Eatable { void eat(); }
```

**D — Dependency Inversion Principle (의존 역전 원칙)**
> 구체 클래스가 아닌 추상(인터페이스)에 의존해야 한다.
```java
// ❌ 구체 클래스에 의존
class UserService { MySQLRepo repo = new MySQLRepo(); }

// ✅ 추상에 의존
class UserService {
    UserRepository repo; // 인터페이스
    UserService(UserRepository repo) { this.repo = repo; }
}
```

### 꼬리 질문
- SOLID를 모두 지키는 것이 항상 좋은가요?
- SOLID 원칙 중 실무에서 가장 자주 위반되는 것은?

### 실무 사례
Spring의 `@Service` + `Repository 인터페이스` 조합은 SRP + DIP를 동시에 준수합니다.
