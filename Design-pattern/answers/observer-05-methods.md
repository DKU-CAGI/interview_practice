## Q. register(), unregister(), notifyObservers() 메서드의 역할을 설명해주세요.

### 핵심 답변
`register()`는 옵저버를 구독 목록에 추가, `unregister()`는 목록에서 제거, `notifyObservers()`는 상태 변화를 모든 옵저버에게 전파합니다.

### 상세 설명

```javascript
class Subject {
    constructor() {
        this.observers = new Set(); // 중복 방지에 Set 사용
    }

    // 구독 등록: Observer를 목록에 추가
    register(observer) {
        if (!observer || typeof observer.update !== 'function') {
            throw new Error('유효한 Observer가 아닙니다.');
        }
        this.observers.add(observer);
    }

    // 구독 해제: Observer를 목록에서 제거
    // 메모리 누수 방지를 위해 반드시 제공해야 함
    unregister(observer) {
        this.observers.delete(observer);
    }

    // 상태 변화 알림: 등록된 모든 Observer의 update() 호출
    notifyObservers(data) {
        this.observers.forEach(observer => {
            try {
                observer.update(data);
            } catch (err) {
                console.error(`Observer 오류:`, err);
                // 하나의 옵저버 오류가 다른 옵저버 알림을 막으면 안 됨
            }
        });
    }
}
```

**각 메서드의 주의사항:**

| 메서드 | 주의사항 |
|--------|----------|
| `register()` | 중복 등록 방지 (Set 사용 권장) |
| `unregister()` | **메모리 누수 방지** — 반드시 구현 |
| `notifyObservers()` | 하나의 Observer 예외가 전체 알림을 막지 않도록 try-catch |

**메모리 누수 문제:**
```javascript
// ❌ unregister 하지 않으면 GC 불가 → 메모리 누수
component.mounted(() => publisher.register(this));
// ✅ 컴포넌트 해제 시 구독 해제
component.unmounted(() => publisher.unregister(this));
```

### 꼬리 질문
- WeakSet을 사용해서 메모리 누수를 자동으로 방지할 수 있나요?
- notifyObservers의 호출 순서가 중요한 경우 어떻게 처리하나요?

### 실무 사례
Vue.js 컴포넌트의 `mounted`/`beforeUnmount` 훅에서 이벤트 구독/해제, React의 `useEffect` cleanup 함수에서 구독 해제가 이 패턴을 따릅니다.
