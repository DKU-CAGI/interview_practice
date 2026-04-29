## Q. 옵저버 패턴에서 Subject와 Observer의 역할을 설명해주세요.

### 핵심 답변
Subject는 상태를 보유하고 옵저버 목록을 관리하며 상태 변화를 알리는 발행자 역할을, Observer는 Subject에 등록하고 알림을 받아 처리하는 구독자 역할을 합니다.

### 상세 설명

**Subject의 책임:**
1. Observer 목록 관리 (`register`, `unregister`)
2. 상태(State) 보유
3. 상태 변화 시 모든 Observer에게 알림 (`notify`)
4. Observer의 구체 타입을 알 필요 없음 (인터페이스에만 의존)

**Observer의 책임:**
1. Subject에 자신을 등록
2. `update()` 메서드 구현 — Subject의 알림을 수신
3. Subject로부터 필요한 데이터 추출

```javascript
// Subject
class NewsPublisher {
    constructor() {
        this.observers = new Set();
        this.latestNews = null;
    }

    register(observer) { this.observers.add(observer); }
    unregister(observer) { this.observers.delete(observer); }

    publishNews(news) {
        this.latestNews = news;
        this.notifyObservers(); // 상태 변화 → 알림
    }

    notifyObservers() {
        this.observers.forEach(observer => observer.update(this.latestNews));
    }
}

// Observer 인터페이스
class NewsObserver {
    update(news) { throw new Error('구현 필요'); }
}

// ConcreteObserver
class EmailSubscriber extends NewsObserver {
    constructor(email) {
        super();
        this.email = email;
    }
    update(news) {
        console.log(`[이메일 발송] ${this.email}: ${news}`);
    }
}

class PushSubscriber extends NewsObserver {
    update(news) {
        console.log(`[푸시 알림] ${news}`);
    }
}

// 사용
const publisher = new NewsPublisher();
const emailSub = new EmailSubscriber('user@example.com');
const pushSub = new PushSubscriber();

publisher.register(emailSub);
publisher.register(pushSub);

publisher.publishNews('중요 업데이트 발생!');
// → [이메일 발송] user@example.com: 중요 업데이트 발생!
// → [푸시 알림] 중요 업데이트 발생!
```

### 꼬리 질문
- Subject가 Observer에게 데이터를 전달하는 "Push 방식"과 Observer가 데이터를 가져오는 "Pull 방식"의 차이는?
- 옵저버가 Subject를 몰라도 되는 설계가 가능한가요?

### 실무 사례
Redux의 `store.subscribe()`, MobX의 observable/reaction, DOM의 CustomEvent 발행/구독이 Subject-Observer 관계를 명확히 보여줍니다.
