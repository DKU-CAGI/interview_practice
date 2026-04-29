## Q. 고차 함수란 무엇인가요?

### 핵심 답변
고차 함수(Higher-Order Function)는 함수를 인수로 받거나 함수를 반환하는 함수입니다. 함수형 프로그래밍의 핵심이며, 일급 객체인 함수의 특성을 활용합니다.

### 상세 설명

**두 가지 형태:**

**1. 함수를 인수로 받는 경우:**
```javascript
// map, filter, reduce가 대표적 고차 함수
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(x => x * 2); // [2, 4, 6, 8, 10]
const evens = numbers.filter(x => x % 2 === 0); // [2, 4]
const sum = numbers.reduce((acc, x) => acc + x, 0); // 15

// 콜백을 받는 함수
function repeat(times, callback) {
    for (let i = 0; i < times; i++) {
        callback(i);
    }
}
repeat(3, i => console.log(i)); // 0, 1, 2
```

**2. 함수를 반환하는 경우 (커링, 클로저):**
```javascript
// 커링(Currying): 다중 인수 함수를 단일 인수 함수 체인으로
const multiply = a => b => a * b;
const double = multiply(2); // 함수 반환
const triple = multiply(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// 데코레이터 역할
function withLogging(fn) {
    return function(...args) {
        console.log(`호출: ${fn.name}(${args})`);
        const result = fn(...args);
        console.log(`결과: ${result}`);
        return result;
    };
}

const loggedAdd = withLogging((a, b) => a + b);
loggedAdd(2, 3); // 호출: ...(2,3) / 결과: 5
```

**React에서의 고차 함수:**
```javascript
// useMemo, useCallback이 고차 함수의 활용
const memoized = useMemo(() => expensiveCalc(data), [data]);

// HOC(Higher-Order Component)
const withAuth = (Component) => (props) => {
    if (!auth.isLoggedIn) return <Redirect to="/login" />;
    return <Component {...props} />;
};
```

### 꼬리 질문
- 커링(Currying)과 부분 적용(Partial Application)의 차이는?
- 고차 함수가 함수형 프로그래밍에서 중요한 이유는?

### 실무 사례
`Array.prototype.sort(compareFn)`, `setTimeout(callback, delay)`, Express의 `app.use(middleware)`, React의 `useEffect(callback, deps)`가 모두 고차 함수입니다.
