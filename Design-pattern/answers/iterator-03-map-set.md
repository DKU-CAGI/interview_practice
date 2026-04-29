## Q. Map과 Set 자료구조에서 이터레이터를 어떻게 사용하나요?

### 핵심 답변
Map과 Set은 모두 이터러블 프로토콜을 구현하여 `for...of`, 스프레드 연산자, 구조 분해 할당 등에서 사용 가능합니다. 각각 `keys()`, `values()`, `entries()` 메서드로 특정 이터레이터를 반환합니다.

### 상세 설명

**Map 이터레이터:**
```javascript
const map = new Map([
    ['name', '홍길동'],
    ['age', 30],
    ['city', '서울'],
]);

// 기본 순회: [key, value] 쌍
for (const [key, value] of map) {
    console.log(`${key}: ${value}`);
}

// keys(), values(), entries()
for (const key of map.keys()) {
    console.log(key); // 'name', 'age', 'city'
}

for (const value of map.values()) {
    console.log(value); // '홍길동', 30, '서울'
}

for (const entry of map.entries()) {
    console.log(entry); // ['name', '홍길동'], ...
}

// 배열로 변환
const keys = [...map.keys()];    // ['name', 'age', 'city']
const entries = [...map.entries()]; // [['name', '홍길동'], ...]
```

**Set 이터레이터:**
```javascript
const set = new Set([1, 2, 3, 3, 2]); // 중복 제거

for (const value of set) {
    console.log(value); // 1, 2, 3 (삽입 순서 유지)
}

// Set의 keys()와 values()는 동일 (값 = 키)
console.log([...set.keys()]);   // [1, 2, 3]
console.log([...set.values()]); // [1, 2, 3]

// 배열 중복 제거
const unique = [...new Set([1, 1, 2, 3, 2])]; // [1, 2, 3]
```

**Object vs Map 순회:**
```javascript
const obj = { a: 1, b: 2 };

// Object는 이터러블이 아님 — Object.entries() 필요
for (const [key, val] of Object.entries(obj)) {
    console.log(key, val);
}

// Map은 직접 순회 가능
const mapObj = new Map(Object.entries(obj));
for (const [key, val] of mapObj) {
    console.log(key, val);
}
```

### 꼬리 질문
- Map이 일반 Object보다 이터레이션에서 유리한 점은 무엇인가요?
- WeakMap과 WeakSet은 왜 이터러블이 아닌가요?

### 실무 사례
Map은 삽입 순서가 보장되고 비문자열 키를 사용할 수 있어, 순서가 중요한 데이터 저장이나 DOM 노드를 키로 사용할 때 Object보다 Map을 선택합니다.
