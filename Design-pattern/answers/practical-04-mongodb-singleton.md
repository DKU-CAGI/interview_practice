## Q. MongoDB에서 싱글톤 패턴을 사용하는 이유는?

### 핵심 답변
MongoDB 연결은 생성 비용이 높고, 연결 풀(Connection Pool)을 애플리케이션 전체가 공유해야 하며, 동시 연결 수에 제한이 있기 때문에 싱글톤으로 연결을 관리합니다.

### 상세 설명

**MongoDB 연결 비용:**
1. TCP 소켓 연결 수립
2. MongoDB 서버와 인증 교환
3. 연결 풀 초기화 (기본 5개 연결)
4. 세션 상태 설정

**싱글톤 없이 매번 새 연결 생성 시 문제:**
```javascript
// ❌ 요청마다 새 연결 생성 — 성능 저하, 연결 수 폭발
app.get('/users', async (req, res) => {
    const client = new MongoClient(uri); // 매번 새 연결!
    await client.connect();
    const users = await client.db().collection('users').find().toArray();
    await client.close(); // 매번 닫음
    res.json(users);
});
```

**싱글톤으로 연결 재사용:**
```javascript
// db.js — 싱글톤 연결
const { MongoClient } = require('mongodb');

class Database {
    static #instance = null;
    #client = null;

    static getInstance() {
        if (!Database.#instance) {
            Database.#instance = new Database();
        }
        return Database.#instance;
    }

    async connect() {
        if (!this.#client) {
            this.#client = new MongoClient(process.env.MONGO_URI, {
                maxPoolSize: 10, // 연결 풀 크기
                minPoolSize: 2,
            });
            await this.#client.connect();
        }
        return this.#client.db('mydb');
    }
}

module.exports = Database.getInstance();
```

**Mongoose에서의 싱글톤:**
```javascript
// mongoose.js — 모듈 레벨 싱글톤
const mongoose = require('mongoose');

mongoose.connect(process.env.MONGO_URI);
// Node.js의 require() 캐싱으로 자동 싱글톤

// 모든 모델에서 동일한 연결 사용
const User = mongoose.model('User', userSchema);
const Order = mongoose.model('Order', orderSchema);
```

**Connection Pool의 역할:**
- 미리 생성된 연결을 재사용 → 지연 최소화
- 최대 연결 수 제한 → MongoDB 서버 보호
- 연결 건강 확인 및 재연결 자동 처리

### 꼬리 질문
- Connection Pool 크기를 어떻게 결정하나요?
- 멀티프로세스(Node.js cluster) 환경에서 MongoDB 연결은 어떻게 관리하나요?

### 실무 사례
Node.js cluster 사용 시 각 worker 프로세스가 독립적인 MongoDB 연결을 가집니다. 연결 풀 크기를 `(최대 연결 수 / worker 수)`로 설정하여 전체 연결 수를 제어합니다.
