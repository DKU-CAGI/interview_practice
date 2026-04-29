## Q. 인터페이스 분리 원칙(ISP)이란 무엇인가요?

### 핵심 답변
ISP는 "클라이언트는 자신이 사용하지 않는 메서드에 의존하도록 강제되어서는 안 된다"는 원칙입니다. 범용 인터페이스 하나보다 특정 목적의 작은 인터페이스 여럿이 낫습니다.

### 상세 설명

**ISP 위반:**
```java
// 너무 큰 인터페이스: 모든 구현체가 불필요한 메서드까지 구현해야 함
interface Animal {
    void eat();
    void sleep();
    void fly();  // 날지 못하는 동물도 구현 강제
    void swim(); // 수영 못하는 동물도 구현 강제
    void run();
}

class Penguin implements Animal {
    void eat() { /* 구현 */ }
    void sleep() { /* 구현 */ }
    void fly() { throw new UnsupportedOperationException(); } // 의미없는 구현
    void swim() { /* 구현 */ }
    void run() { /* 구현 */ }
}
```

**ISP 준수:**
```java
// 역할별 인터페이스 분리
interface Eatable { void eat(); }
interface Sleepable { void sleep(); }
interface Flyable { void fly(); }
interface Swimmable { void swim(); }

// 각 클래스가 필요한 인터페이스만 구현
class Eagle implements Eatable, Sleepable, Flyable {
    void eat() { /* */ }
    void sleep() { /* */ }
    void fly() { /* */ }
    // swim() 구현 강제 없음
}

class Penguin implements Eatable, Sleepable, Swimmable {
    void eat() { /* */ }
    void sleep() { /* */ }
    void swim() { /* */ }
    // fly() 구현 강제 없음
}
```

**JavaScript에서의 ISP:**
```javascript
// TypeScript 인터페이스 분리
interface Printable { print(): void; }
interface Scannable { scan(): void; }
interface Faxable { fax(): void; }

// 단순 프린터는 Printable만 구현
class SimplePrinter implements Printable {
    print() { console.log('출력'); }
}

// 복합기는 모두 구현
class MultiFunctionPrinter implements Printable, Scannable, Faxable {
    print() { /* */ }
    scan() { /* */ }
    fax() { /* */ }
}
```

**ISP의 이점:**
- 불필요한 의존성 제거
- 변경 영향 범위 최소화
- 인터페이스 명확성 향상
- 테스트 용이성 향상

### 꼬리 질문
- ISP와 SRP의 차이점은 무엇인가요?
- 마이크로서비스에서 ISP는 어떻게 적용되나요?

### 실무 사례
Spring Data의 `CrudRepository`, `JpaRepository`는 ISP를 적용한 계층적 인터페이스입니다. 기본 CRUD만 필요하면 `CrudRepository`를, 페이징이 필요하면 `JpaRepository`를 사용합니다.
