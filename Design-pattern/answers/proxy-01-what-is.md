## Q. 프록시 패턴이란 무엇이며, 어떤 용도로 사용되나요?

### 핵심 답변
프록시 패턴은 대상 객체(Real Object)에 대한 접근을 제어하는 대리 객체(Proxy)를 제공하는 구조 패턴입니다. 원본 객체를 직접 수정하지 않고 접근 제어, 캐싱, 로깅 등 부가 기능을 추가합니다.

### 상세 설명

**주요 용도:**
| 종류 | 설명 |
|------|------|
| 보호 프록시 | 접근 권한 제어 |
| 가상 프록시 | 무거운 객체의 지연 로딩(Lazy Loading) |
| 캐싱 프록시 | 연산 결과 캐싱 |
| 로깅 프록시 | 요청/응답 로깅 |
| 원격 프록시 | 원격 객체 로컬 접근 (RPC, RMI) |

```javascript
// 보호 프록시 예시
const adminService = {
    deleteUser(id) { console.log(`사용자 ${id} 삭제`); },
    getUsers() { return ['user1', 'user2']; },
};

const protectedService = new Proxy(adminService, {
    get(target, prop) {
        if (prop === 'deleteUser' && !currentUser.isAdmin) {
            throw new Error('권한 없음');
        }
        return target[prop];
    }
});
```

**가상 프록시 (지연 로딩):**
```javascript
class ImageProxy {
    constructor(url) {
        this.url = url;
        this.realImage = null;
    }

    display() {
        if (!this.realImage) {
            this.realImage = new RealImage(this.url); // 실제 로딩은 필요할 때
        }
        this.realImage.display();
    }
}
```

### 장단점
- **장점**: 원본 객체 수정 없이 기능 추가, OCP 준수, 관심사 분리
- **단점**: 응답 지연 가능성, 코드 복잡도 증가

### 꼬리 질문
- 프록시 패턴과 데코레이터 패턴의 차이는 무엇인가요?
- 프록시 패턴과 실제 프록시 서버는 어떤 공통점이 있나요?

### 실무 사례
Spring AOP(Aspect-Oriented Programming)가 프록시 패턴으로 구현됩니다. `@Transactional`, `@Cacheable` 어노테이션이 원본 메서드를 프록시로 감싸 트랜잭션/캐싱을 추가합니다.
