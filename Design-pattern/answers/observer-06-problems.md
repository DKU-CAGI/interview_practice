## Q. 옵저버 패턴을 사용할 때 발생할 수 있는 문제점은 무엇인가요?

### 핵심 답변
메모리 누수, 예상치 못한 업데이트 순서, 연쇄 업데이트(Cascading), 성능 저하 등의 문제가 발생할 수 있습니다.

### 상세 설명

**1. 메모리 누수 (가장 흔한 문제)**
```javascript
// 컴포넌트가 제거됐지만 Subject가 Observer 참조를 계속 보유
class Component {
    constructor(subject) {
        subject.register(this); // 등록
        // 컴포넌트 소멸 시 unregister를 안 하면 GC 불가
    }
}
// 해결: 소멸 시 반드시 subject.unregister(this) 호출
```

**2. 업데이트 순서 불확실성**
```javascript
subject.register(observerA);
subject.register(observerB);
// A가 먼저 호출된다는 보장 없음 (Set의 삽입 순서)
// B의 update()가 A의 결과에 의존하면 버그 발생 가능
```

**3. 연쇄 업데이트 (Cascading Updates)**
```javascript
// ObserverA의 update()가 Subject를 변경 → 다시 notify → 무한 루프
observerA.update = (data) => {
    subject.setState(data.value + 1); // ← 다시 notifyObservers() 발생!
}
```

**4. 성능 문제**
- 옵저버 수가 많거나 업데이트가 빈번할 때 O(n) 알림 비용
- 불필요한 업데이트: 관심 없는 상태 변화에도 알림 수신

**5. 디버깅 어려움**
- 비동기 알림 시 호출 스택 추적 어려움
- 어떤 Subject가 어떤 Observer를 업데이트했는지 추적 복잡

**해결 방법:**
- WeakRef/WeakSet으로 메모리 누수 방지
- 이벤트 필터링으로 불필요한 알림 감소
- 디바운스/스로틀 적용
- 이벤트 이름 기반 세분화된 구독

### 꼬리 질문
- 옵저버 수가 수백만 개일 때 성능을 어떻게 개선할 수 있나요?
- RxJS의 Observable은 이 문제들을 어떻게 해결하나요?

### 실무 사례
React의 `useEffect` 클린업 함수, Vue의 `onUnmounted` 훅은 메모리 누수 방지를 위해 구독 해제를 강제하는 구조를 제공합니다.
