## Q. 선언형 프로그래밍과 함수형 프로그래밍의 차이는?

### 핵심 답변
선언형 프로그래밍은 "무엇을 할지"를 기술하고(How가 아닌 What), 함수형 프로그래밍은 선언형의 한 형태로 순수 함수와 불변성을 통해 부수효과(Side Effect)를 최소화합니다.

### 상세 설명

**명령형 vs 선언형:**
```javascript
const numbers = [1, 2, 3, 4, 5];

// 명령형(Imperative): "어떻게" 할지 기술
const evens = [];
for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] % 2 === 0) evens.push(numbers[i]);
}

// 선언형(Declarative): "무엇을" 원하는지 기술
const evens = numbers.filter(n => n % 2 === 0);
```

**함수형 프로그래밍의 핵심 특징:**
```javascript
// 1. 순수 함수 (같은 입력 → 같은 출력, 부수효과 없음)
const double = x => x * 2; // 순수 함수
const addToArray = (arr, item) => [...arr, item]; // 불변성 유지

// 2. 불변성 (원본 변경 없음)
const original = [1, 2, 3];
const doubled = original.map(x => x * 2); // original 유지
// [2, 4, 6], original은 [1, 2, 3]

// 3. 함수 합성
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);
const process = compose(
    x => x * 2,
    x => x + 1,
    x => x - 3
);
process(10); // (10-3+1)*2 = 16
```

**비교:**
| | 명령형 | 선언형 | 함수형 |
|---|---|---|---|
| 초점 | 어떻게 | 무엇을 | 무엇을 (수학적) |
| 상태 변경 | 허용 | 숨김 | 금지 |
| 대표 언어 | C, Java | SQL, HTML | Haskell, Elm |
| JS 지원 | for loop | `filter`, `map` | 클로저, 고차함수 |

### 꼬리 질문
- 함수형 프로그래밍에서 모나드(Monad)란 무엇인가요?
- JavaScript는 왜 멀티 패러다임 언어라고 불리나요?

### 실무 사례
React의 `useState`, `useReducer`, Redux의 리듀서 함수가 함수형 프로그래밍 원칙을 따릅니다. SQL은 대표적인 선언형 언어입니다.
