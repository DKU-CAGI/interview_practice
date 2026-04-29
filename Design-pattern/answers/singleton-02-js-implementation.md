## Q. 싱글톤 패턴을 JavaScript에서 구현하는 방법을 설명해주세요.

### 핵심 답변
JavaScript에서는 ES6 클래스의 static 프로퍼티, 클로저(IIFE), 또는 객체 리터럴을 활용해 싱글톤을 구현합니다.

### 상세 설명

**1. ES6 클래스 + static 프로퍼티 (가장 명시적)**
```javascript
class DatabaseConnection {
  static #instance = null;

  static getInstance() {
    if (!DatabaseConnection.#instance) {
      DatabaseConnection.#instance = new DatabaseConnection();
    }
    return DatabaseConnection.#instance;
  }

  constructor() {
    if (DatabaseConnection.#instance) {
      throw new Error('getInstance()를 사용하세요.');
    }
    this.connection = 'connected';
  }
}

const db1 = DatabaseConnection.getInstance();
const db2 = DatabaseConnection.getInstance();
console.log(db1 === db2); // true
```

**2. 클로저 + IIFE (모듈 패턴)**
```javascript
const Singleton = (() => {
  let instance;

  function createInstance() {
    return { id: Math.random() };
  }

  return {
    getInstance() {
      if (!instance) instance = createInstance();
      return instance;
    }
  };
})();
```

**3. 객체 리터럴 (JS에서 가장 단순)**
```javascript
const config = {
  host: 'localhost',
  port: 3000,
};
// 객체 리터럴 자체가 유일한 인스턴스
```

**4. Node.js 모듈 캐싱 활용**
```javascript
// db.js
class DB { /* ... */ }
module.exports = new DB(); // require() 캐싱으로 자동 싱글톤
```

### 장단점
- **장점**: 간결한 구현, ES6 모듈 시스템과 자연스럽게 통합
- **단점**: 생성자 직접 호출 시 다중 인스턴스 생성 가능성

### 꼬리 질문
- Node.js 모듈 캐싱이 싱글톤처럼 동작하는 이유는 무엇인가요?
- TypeScript에서 싱글톤 구현 시 달라지는 점은 무엇인가요?

### 실무 사례
Node.js에서 `module.exports`로 내보낸 객체는 `require()` 캐싱으로 인해 같은 프로세스 내에서는 자동으로 싱글톤처럼 동작합니다.
