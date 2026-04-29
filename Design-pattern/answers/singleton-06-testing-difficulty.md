## Q. 싱글톤 패턴이 테스트하기 어려운 이유는 무엇인가요?

### 핵심 답변
싱글톤은 전역 상태를 공유하기 때문에 테스트 간 상태 격리가 불가능하고, Mock 객체로 교체하기 어려워 단위 테스트를 방해합니다.

### 상세 설명

**문제 1: 테스트 간 상태 오염**
```javascript
// 테스트 A
const cache = CacheManager.getInstance();
cache.set('key', 'testValueA');

// 테스트 B (같은 인스턴스 공유!)
const cache = CacheManager.getInstance();
console.log(cache.get('key')); // 'testValueA' ← 테스트 A의 잔재
```

**문제 2: Mock 교체 불가**
```javascript
class UserService {
    getUser(id) {
        const db = Database.getInstance(); // 하드코딩된 의존성
        return db.query(`SELECT * FROM users WHERE id = ${id}`);
    }
}
// Database.getInstance()를 Mock으로 교체할 방법이 없음
```

**문제 3: 테스트 순서 의존성**
테스트가 실행 순서에 따라 다른 결과를 내어 테스트의 독립성(Isolation) 원칙 위반

**해결 방법:**
1. **의존성 주입(DI)으로 전환**: 싱글톤을 생성자로 주입받아 테스트 시 Mock 교체
2. **리셋 메서드 제공**: 테스트 전용 `resetInstance()` 메서드
3. **인터페이스 추상화**: 구체 클래스 대신 인터페이스에 의존

```javascript
// DI로 해결
class UserService {
    constructor(db) { // 의존성 주입
        this.db = db;
    }
    getUser(id) { return this.db.query(id); }
}

// 테스트 시 Mock 주입
const mockDb = { query: jest.fn().mockReturnValue({id: 1}) };
const service = new UserService(mockDb);
```

### 꼬리 질문
- 싱글톤을 사용하면서도 테스트 가능하게 설계하려면 어떻게 해야 하나요?
- DI 컨테이너가 싱글톤 테스트 문제를 어떻게 해결하나요?

### 실무 사례
Jest에서 `jest.resetModules()`로 모듈 캐시를 초기화하거나, `jest.mock()`으로 모듈 자체를 교체하여 싱글톤 테스트 문제를 우회합니다.
