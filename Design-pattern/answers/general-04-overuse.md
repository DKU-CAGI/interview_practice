## Q. 디자인 패턴을 너무 많이 사용하면 어떤 문제가 발생할 수 있나요?

### 핵심 답변
불필요한 패턴 남용은 오버엔지니어링(Over-engineering)으로 이어져 코드 복잡성 증가, 가독성 저하, 성능 저하, 유지보수 어려움을 유발합니다.

### 상세 설명

**오버엔지니어링의 예:**
```javascript
// ❌ 단순한 문자열 반환에 팩토리 + 전략 패턴 남용
class GreetingStrategyFactory {
    static create(type) {
        return type === 'formal' ? new FormalGreeting() : new CasualGreeting();
    }
}
class FormalGreeting { greet(name) { return `안녕하세요, ${name}님`; } }
class CasualGreeting { greet(name) { return `안녕 ${name}`; } }

const greeter = GreetingStrategyFactory.create('formal');
greeter.greet('홍길동');

// ✅ 이렇게 하면 충분
const greet = (formal, name) =>
    formal ? `안녕하세요, ${name}님` : `안녕 ${name}`;
```

**발생하는 문제:**

**1. 코드 복잡성 증가**
- 클래스와 인터페이스 수 폭발
- 단순한 작업도 여러 파일을 거쳐야 함

**2. 가독성 저하**
- 새 팀원이 코드를 이해하는 데 시간 소요
- "이 코드가 왜 이렇게 복잡하지?" 의문

**3. YAGNI 원칙 위반**
> YAGNI: "You Aren't Gonna Need It" — 지금 필요하지 않은 것을 미리 만들지 말라

**4. 유지보수 어려움**
- 과도한 추상화 = 실제 로직 파악 어려움
- 버그 추적 시 여러 레이어를 거쳐야 함

**올바른 패턴 적용 기준 (Rule of Three):**
```
1. 처음 구현: 그냥 작성
2. 두 번째 유사 코드: 중복을 인지
3. 세 번째 유사 코드: 패턴/추상화 적용
```

**리팩토링 vs 선제적 추상화:**
- 기존 코드를 패턴으로 리팩토링 → ✅ 올바른 접근
- 미래를 위해 미리 패턴 적용 → ⚠️ 조심

### 꼬리 질문
- KISS 원칙과 DRY 원칙을 디자인 패턴에 어떻게 적용하나요?
- 과도한 추상화를 리팩토링하는 방법은?

### 실무 사례
"Simple Made Easy" (Rich Hickey) — 단순한 코드가 쉬운 코드보다 낫습니다. 패턴은 복잡성을 관리하는 도구이지, 단순한 코드를 복잡하게 만드는 도구가 아닙니다.
