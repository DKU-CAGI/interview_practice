## Q. 이벤트 기반 시스템과 옵저버 패턴의 관계를 설명해주세요.

### 핵심 답변
이벤트 기반 시스템은 옵저버 패턴을 발전시킨 형태입니다. 옵저버 패턴의 Subject-Observer 직접 참조 구조를 이벤트 버스(Event Bus)를 통해 더 느슨하게 분리한 것입니다.

### 상세 설명

**옵저버 패턴의 한계:**
```
Subject → Observer (직접 참조, 둘 다 알고 있어야 함)
```

**이벤트 기반 시스템:**
```
Publisher → [Event Bus] → Subscriber (서로를 모름)
```

**비교:**
| | 옵저버 패턴 | 이벤트 기반 |
|---|---|---|
| 결합도 | Subject가 Observer 목록 보유 | 중간 매개체(Event Bus) 사용 |
| 방향성 | 1:N | N:M 가능 |
| 동기성 | 보통 동기 | 동기/비동기 모두 가능 |
| 이벤트 타입 | 단일 | 이름으로 분류 |

**Node.js EventEmitter (이벤트 기반의 예):**
```javascript
const EventEmitter = require('events');
const eventBus = new EventEmitter();

// Subscriber (Observer)
eventBus.on('user:created', (user) => {
    console.log(`환영 이메일 발송: ${user.email}`);
});

eventBus.on('user:created', (user) => {
    console.log(`추천인 포인트 지급`);
});

// Publisher (Subject)
// Subject가 Observer를 직접 모름
eventBus.emit('user:created', { email: 'new@example.com' });
```

**pub/sub 패턴과의 차이:**
- 옵저버 패턴: Subject가 Observer를 직접 알고 있음
- pub/sub 패턴: 메시지 브로커(Kafka, RabbitMQ 등)를 통해 완전한 분리

### 꼬리 질문
- pub/sub 패턴과 옵저버 패턴의 차이를 설명해주세요.
- Node.js의 비동기 이벤트 루프와 EventEmitter의 관계는?

### 실무 사례
마이크로서비스 아키텍처에서 Kafka/RabbitMQ를 이벤트 버스로 사용하는 것이 옵저버 패턴의 분산 시스템 버전입니다. 같은 서버 내에서는 Node.js EventEmitter를 사용합니다.
