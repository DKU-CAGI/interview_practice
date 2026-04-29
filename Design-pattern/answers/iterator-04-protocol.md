## Q. 이터레이터 프로토콜과 이터러블 객체의 차이는 무엇인가요?

### 핵심 답변
이터러블(Iterable)은 `[Symbol.iterator]()`를 구현하여 이터레이터를 반환하는 객체이고, 이터레이터(Iterator)는 `next()` 메서드로 순차적으로 값을 반환하는 객체입니다. 이터러블이 이터레이터를 만들어내는 공장 역할입니다.

### 상세 설명

**이터러블(Iterable):**
- 조건: `[Symbol.iterator]()` 메서드 보유
- 역할: 이터레이터를 생성하는 공장
- 예: Array, String, Map, Set, 제너레이터

**이터레이터(Iterator):**
- 조건: `{ value, done }` 형태를 반환하는 `next()` 메서드 보유
- 역할: 실제 순회를 수행하는 커서

```javascript
const array = [1, 2, 3]; // ← 이터러블

// [Symbol.iterator]() 호출 → 이터레이터 반환
const iterator = array[Symbol.iterator](); // ← 이터레이터

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

**자기 자신을 반환하는 이터레이터 (Well-formed 이터레이터):**
```javascript
// 많은 내장 이터레이터는 자기 자신도 이터러블
const iter = [1, 2, 3][Symbol.iterator]();
console.log(iter[Symbol.iterator]() === iter); // true
// → for...of에 이터레이터를 직접 넣을 수 있음
for (const v of iter) console.log(v);
```

**직접 구현 (이터러블이자 이터레이터):**
```javascript
class Fibonacci {
    constructor(limit) { this.limit = limit; }

    [Symbol.iterator]() {
        let [prev, curr] = [0, 1];
        let count = 0;
        const limit = this.limit;
        return {
            next() {
                if (count++ < limit) {
                    [prev, curr] = [curr, prev + curr];
                    return { value: prev, done: false };
                }
                return { value: undefined, done: true };
            }
        };
    }
}

console.log([...new Fibonacci(7)]); // [1, 1, 2, 3, 5, 8, 13]
```

### 꼬리 질문
- 제너레이터 함수가 이터레이터를 반환하는 이유는 무엇인가요?
- 이터레이터에서 `return()`, `throw()` 메서드의 역할은?

### 실무 사례
RxJS의 Observable은 이터러블/이터레이터 패턴을 비동기로 확장한 것입니다. `for await...of`로 비동기 이터레이터를 소비합니다.
