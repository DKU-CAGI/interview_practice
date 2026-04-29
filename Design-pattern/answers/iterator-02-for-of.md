## Q. JavaScript에서 for...of 문과 이터레이터의 관계를 설명해주세요.

### 핵심 답변
`for...of`는 이터러블 프로토콜을 따르는 객체를 순회하는 문법입니다. 내부적으로 객체의 `[Symbol.iterator]()` 메서드를 호출하여 이터레이터를 가져오고, `next()`를 반복 호출하여 `done: true`가 될 때까지 순회합니다.

### 상세 설명

**for...of의 내부 동작:**
```javascript
const iterable = [1, 2, 3];

// for...of가 내부적으로 하는 일
const iterator = iterable[Symbol.iterator](); // 이터레이터 획득
let result = iterator.next();
while (!result.done) {
    console.log(result.value); // 값 처리
    result = iterator.next();
}

// 위 코드와 동일
for (const value of [1, 2, 3]) {
    console.log(value);
}
```

**for...of vs for...in 차이:**
| | for...of | for...in |
|---|---|---|
| 대상 | 이터러블 객체 | 객체의 열거 가능 프로퍼티 |
| 반환 | 값(value) | 키(key) |
| 프로토타입 | 포함 안 함 | 포함 (hasOwnProperty 필요) |
| 배열 | 요소 값 순회 | 인덱스 문자열 순회 |

```javascript
const arr = [10, 20, 30];

for (const val of arr) console.log(val); // 10, 20, 30
for (const key in arr) console.log(key); // '0', '1', '2'

// 이터러블이 아닌 일반 객체는 for...of 불가
const obj = { a: 1, b: 2 };
for (const x of obj) {} // TypeError: obj is not iterable
```

**이터러블이 가능한 타입:**
- Array, String, Map, Set, TypedArray
- `arguments` 객체
- DOM NodeList
- 제너레이터 함수 반환값

### 꼬리 질문
- `for...of`에서 `break`나 `return`을 사용하면 이터레이터에 어떤 영향을 미치나요? (iterator.return() 호출)
- 비동기 이터레이터(`for await...of`)는 어떤 상황에서 사용하나요?

### 실무 사례
`for await...of`로 스트림 데이터나 페이지네이션 API를 순차 처리하는 패턴이 Node.js에서 자주 사용됩니다.
