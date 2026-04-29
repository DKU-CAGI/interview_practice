## Q. 옵저버 패턴이란 무엇이며, 어떤 상황에서 사용하나요?

### 핵심 답변
옵저버 패턴은 한 객체(Subject)의 상태가 변화하면 이를 관찰하는 여러 객체(Observer)에 자동으로 알림을 전파하는 행동 패턴입니다. 1:N 의존 관계를 느슨하게 구성할 때 사용합니다.

### 상세 설명

**핵심 구조:**
- **Subject(발행자)**: 상태를 보유하고 옵저버를 관리, 상태 변화 시 알림 발송
- **Observer(구독자)**: Subject의 상태 변화를 수신하고 반응

**사용하는 상황:**
- 한 객체의 변화가 다른 여러 객체에 영향을 미칠 때
- 어떤 객체가 변화를 구독할지 사전에 알 수 없을 때
- 느슨한 결합이 필요할 때 (Subject는 Observer의 구체적인 타입을 모름)

```javascript
class EventEmitter {
    constructor() {
        this.listeners = {};
    }

    on(event, callback) {
        if (!this.listeners[event]) {
            this.listeners[event] = [];
        }
        this.listeners[event].push(callback);
    }

    off(event, callback) {
        this.listeners[event] = this.listeners[event]
            .filter(cb => cb !== callback);
    }

    emit(event, data) {
        if (this.listeners[event]) {
            this.listeners[event].forEach(cb => cb(data));
        }
    }
}
```

**실제 적용 상황:**
- UI 이벤트 처리 (버튼 클릭 → 여러 컴포넌트 업데이트)
- 데이터 변화 감지 (Vue.js 반응형 시스템)
- 뉴스레터/알림 시스템
- 소셜 미디어 팔로우 시스템

### 장단점
- **장점**: 느슨한 결합, OCP 준수, 런타임 동적 구독
- **단점**: 예상치 못한 업데이트 순서, 메모리 누수(구독 해제 미흡 시)

### 꼬리 질문
- 옵저버 패턴과 이벤트 기반 시스템의 차이는?
- 옵저버가 많을 때 성능 문제를 어떻게 해결하나요?

### 실무 사례
Node.js의 `EventEmitter`, DOM의 `addEventListener`, RxJS의 Observable, Vue.js의 반응형 시스템이 모두 옵저버 패턴 기반입니다.
