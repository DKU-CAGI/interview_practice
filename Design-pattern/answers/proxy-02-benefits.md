## Q. 프록시 객체를 사용하는 이점은 무엇인가요?

### 핵심 답변
프록시 객체는 원본 객체를 변경하지 않고 접근 제어, 유효성 검사, 캐싱, 로깅 등 횡단 관심사(Cross-Cutting Concerns)를 분리하여 추가할 수 있습니다.

### 상세 설명

**1. 접근 제어 (보안)**
```javascript
const secureUser = new Proxy(user, {
    get(target, prop) {
        if (prop === 'password') return '***'; // 민감 정보 차단
        return target[prop];
    }
});
```

**2. 유효성 검사 (입력 검증)**
```javascript
const validatedUser = new Proxy({}, {
    set(target, prop, value) {
        if (prop === 'age' && (typeof value !== 'number' || value < 0)) {
            throw new TypeError('나이는 양수 숫자여야 합니다.');
        }
        target[prop] = value;
        return true;
    }
});
validatedUser.age = -1; // TypeError 발생
```

**3. 캐싱 (성능 최적화)**
```javascript
function createCachedApi(api) {
    const cache = new Map();
    return new Proxy(api, {
        get(target, method) {
            return async (...args) => {
                const key = `${method}:${JSON.stringify(args)}`;
                if (cache.has(key)) return cache.get(key);
                const result = await target[method](...args);
                cache.set(key, result);
                return result;
            };
        }
    });
}
```

**4. 로깅/모니터링**
```javascript
const loggedService = new Proxy(service, {
    get(target, prop) {
        return (...args) => {
            console.log(`[호출] ${prop}(${JSON.stringify(args)})`);
            const result = target[prop](...args);
            console.log(`[결과] ${JSON.stringify(result)}`);
            return result;
        };
    }
});
```

**5. 원본 불변성 유지**
```javascript
const immutable = new Proxy(obj, {
    set() { throw new Error('읽기 전용'); },
    deleteProperty() { throw new Error('삭제 불가'); },
});
```

### 꼬리 질문
- 프록시와 Object.defineProperty의 차이는 무엇인가요?
- 프록시 사용 시 성능 저하를 최소화하는 방법은?

### 실무 사례
Vue.js 3의 반응형 시스템, Immer.js의 불변 상태 관리, Jest의 mock 객체가 모두 JavaScript `Proxy`를 활용합니다.
