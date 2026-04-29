## Q. JavaScript에서 Proxy 객체를 사용하는 방법을 설명해주세요.

### 핵심 답변
JavaScript의 `Proxy`는 `new Proxy(target, handler)` 형태로 생성하며, `handler`에 `get`, `set`, `has` 등의 트랩(trap)을 정의하여 객체 기본 동작을 가로채고 커스터마이징합니다.

### 상세 설명

**기본 문법:**
```javascript
const proxy = new Proxy(target, handler);
```
- `target`: 프록시할 원본 객체
- `handler`: 트랩(가로채기 동작) 정의 객체

**주요 트랩:**
```javascript
const handler = {
    // 프로퍼티 읽기 가로채기
    get(target, prop, receiver) {
        console.log(`GET: ${prop}`);
        return Reflect.get(target, prop, receiver);
    },

    // 프로퍼티 쓰기 가로채기
    set(target, prop, value, receiver) {
        console.log(`SET: ${prop} = ${value}`);
        return Reflect.set(target, prop, value, receiver);
    },

    // in 연산자 가로채기
    has(target, prop) {
        console.log(`HAS: ${prop}`);
        return Reflect.has(target, prop);
    },

    // delete 가로채기
    deleteProperty(target, prop) {
        console.log(`DELETE: ${prop}`);
        return Reflect.deleteProperty(target, prop);
    },

    // 함수 호출 가로채기 (target이 함수일 때)
    apply(target, thisArg, args) {
        console.log(`CALL: ${target.name}(${args})`);
        return Reflect.apply(target, thisArg, args);
    }
};

const user = { name: '홍길동', age: 30 };
const proxyUser = new Proxy(user, handler);

proxyUser.name;           // GET: name
proxyUser.email = 'a@b'; // SET: email = a@b
'name' in proxyUser;      // HAS: name
```

**Reflect와 함께 사용하는 이유:**
`Reflect`는 Proxy 트랩과 1:1 대응하는 메서드를 제공합니다. 트랩에서 `return target[prop]` 대신 `return Reflect.get(target, prop, receiver)`를 사용하면 prototype 체인과 getter 동작을 올바르게 처리합니다.

### 꼬리 질문
- Proxy와 Object.defineProperty의 주요 차이점은 무엇인가요?
- Proxy의 성능 비용은 어느 정도인가요?

### 실무 사례
Vue 3의 `reactive()`, Mobx의 `observable()`, Immer.js가 내부적으로 Proxy를 사용합니다. Object.defineProperty 기반의 Vue 2 대비 Vue 3이 Proxy로 전환한 이유는 배열 변경 감지와 새 프로퍼티 추가 감지가 가능해졌기 때문입니다.
