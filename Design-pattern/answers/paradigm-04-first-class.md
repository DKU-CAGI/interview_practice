## Q. 일급 객체의 조건은 무엇인가요?

### 핵심 답변
일급 객체(First-Class Object)는 변수에 할당 가능하고, 함수의 인수로 전달 가능하며, 함수의 반환값으로 사용 가능한 객체입니다. JavaScript에서 함수는 일급 객체입니다.

### 상세 설명

**일급 객체의 3가지 조건:**

**1. 변수에 할당 가능**
```javascript
const greet = function(name) {
    return `안녕, ${name}!`;
};

const sayHello = greet; // 변수에 할당
console.log(sayHello('홍길동')); // 안녕, 홍길동!
```

**2. 함수의 인수로 전달 가능**
```javascript
function executeWith(value, fn) {
    return fn(value); // 함수를 인수로 받음
}

const result = executeWith(5, x => x * 2); // 10
[1, 2, 3].map(x => x * 2); // 콜백이 인수
```

**3. 함수의 반환값으로 사용 가능**
```javascript
function createMultiplier(factor) {
    return (x) => x * factor; // 함수 반환
}

const double = createMultiplier(2);
const triple = createMultiplier(3);
console.log(double(5)); // 10
console.log(triple(5)); // 15
```

**추가 특성 (일급 객체이면 가능한 것들):**
```javascript
// 배열/객체에 저장
const operations = [
    (a, b) => a + b,
    (a, b) => a - b,
    (a, b) => a * b,
];

// 프로퍼티와 메서드 보유
function myFunc() {}
myFunc.description = '내 함수'; // 프로퍼티 추가 가능
console.log(myFunc.name); // 'myFunc'
console.log(myFunc.length); // 매개변수 수
```

**일급 함수가 가능한 것들:**
- 고차 함수 구현
- 클로저
- 커링
- 함수 합성
- 콜백 패턴
- 이벤트 핸들러

**언어별 비교:**
| 언어 | 함수가 일급 객체? |
|------|-----------------|
| JavaScript | ✅ |
| Python | ✅ |
| Java (Java 8+ lambda) | ✅ (제한적) |
| C (함수 포인터만) | ⚠️ |

### 꼬리 질문
- JavaScript에서 함수가 일급 객체이기 때문에 가능한 패턴은?
- 클로저와 일급 함수의 관계는?

### 실무 사례
React의 이벤트 핸들러, Node.js의 콜백, Express 미들웨어가 모두 함수가 일급 객체이기에 가능한 패턴입니다.
