## Q. 데이터베이스 연결 시 싱글톤 패턴을 사용하는 이유는 무엇인가요?

### 핵심 답변
데이터베이스 연결은 생성 비용이 높고, DB 서버의 최대 동시 연결 수에 제한이 있으며, Connection Pool을 단일 진입점으로 관리해야 하기 때문에 싱글톤 패턴을 사용합니다.

### 상세 설명

**DB 연결이 비싼 이유:**
1. TCP 연결 수립 (3-way handshake)
2. 서버와의 인증/인가 교환
3. 연결 버퍼 메모리 할당
4. 세션 상태 초기화

**싱글톤으로 얻는 이점:**
- **연결 재사용**: 매 요청마다 새 연결을 맺지 않아 지연 감소
- **연결 수 제한 준수**: DB 서버의 `max_connections` 초과 방지
- **Connection Pool 일원화**: 풀을 하나로 관리해 효율 극대화

```javascript
const mongoose = require('mongoose');

class Database {
  static #instance = null;

  static getInstance() {
    if (!Database.#instance) {
      Database.#instance = new Database();
    }
    return Database.#instance;
  }

  async connect(uri) {
    if (mongoose.connection.readyState === 0) {
      await mongoose.connect(uri);
    }
  }
}

module.exports = Database.getInstance();
```

**Connection Pool과의 관계:**
싱글톤으로 Connection Pool 관리자를 하나만 두고, 개별 쿼리는 풀에서 연결을 빌려 사용한 후 반납합니다.

### 장단점
- **장점**: 연결 비용 절감, 리소스 효율 극대화
- **단점**: 단일 장애점(SPOF), 멀티프로세스 환경(Node.js cluster)에서는 프로세스별 별도 연결 필요

### 꼬리 질문
- Connection Pool과 싱글톤의 차이점은 무엇인가요?
- MongoDB와 MySQL에서 싱글톤을 사용하는 방식에 차이가 있나요?

### 실무 사례
Mongoose, Sequelize, TypeORM 모두 초기화 후 단일 연결 인스턴스를 전역으로 재사용하도록 설계되어 있습니다. `mongoose.connect()`를 앱 시작 시 한 번만 호출하고 이후 모든 모델에서 해당 연결을 공유합니다.
