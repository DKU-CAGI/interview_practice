## Q. 순수 함수란 무엇이며, 어떤 특징이 있나요?

### 핵심 답변
순수 함수는 같은 입력에 항상 같은 출력을 반환하고, 함수 외부의 상태를 변경하지 않는(부수효과 없는) 함수입니다.

### 상세 설명

**두 가지 조건:**
1. **참조 투명성(Referential Transparency)**: 같은 인자 → 항상 같은 반환값
2. **부수효과 없음(No Side Effects)**: 외부 상태 변경 없음

**순수 함수 vs 비순수 함수:**
```javascript
// ✅ 순수 함수
function add(a, b) {
    return a + b; // 같은 입력 → 항상 같은 출력
}

function double(arr) {
    return arr.map(x => x * 2); // 원본 변경 없음
}

// ❌ 비순수 함수: 외부 상태에 의존
let count = 0;
function increment() {
    return ++count; // 외부 변수 변경 (부수효과)
}

// ❌ 비순수 함수: 같은 입력 다른 출력
function getCurrentTime() {
    return new Date(); // 호출 시점마다 다름
}

// ❌ 비순수 함수: 원본 변경
function pushItem(arr, item) {
    arr.push(item); // 입력 변경 (부수효과)
    return arr;
}
```

**부수효과의 예:**
- 전역 변수/외부 변수 변경
- 콘솔 출력, 로그
- 네트워크 요청
- 파일 I/O
- DOM 조작
- 날짜/랜덤 값 사용

**순수 함수의 이점:**
- **예측 가능성**: 같은 입력 → 같은 결과 보장
- **테스트 용이성**: 외부 의존성 없이 단독 테스트
- **메모이제이션**: 결과 캐싱 가능
- **병렬 처리**: 상태 공유 없어 Race Condition 없음

### 꼬리 질문
- Redux 리듀서가 순수 함수여야 하는 이유는?
- 부수효과를 완전히 없앨 수 없는 경우 어떻게 처리하나요? (Monad, Effect)

### 실무 사례
Redux에서 리듀서는 `(state, action) => newState` 형태의 순수 함수입니다. 부수효과(API 호출 등)는 미들웨어(Redux-Saga, Redux-Thunk)에서 처리합니다.
