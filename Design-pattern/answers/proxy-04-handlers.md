## Q. get(), set(), has() 핸들러의 역할을 설명해주세요.

### 핵심 답변
`get()`은 프로퍼티 읽기를, `set()`은 프로퍼티 쓰기를, `has()`는 `in` 연산자를 가로채 커스텀 로직을 실행합니다.

### 상세 설명

**get 트랩: 프로퍼티 읽기 가로채기**
```javascript
// 서명: get(target, property, receiver)
const withDefault = new Proxy({}, {
    get(target, prop) {
        return prop in target ? target[prop] : `기본값(${prop})`;
    }
});

withDefault.name;   // '기본값(name)'
withDefault.x = 1;
withDefault.x;     // 1
```
활용: 기본값 제공, 접근 로깅, 읽기 전용 필드 보호, 번역/다국어 처리

**set 트랩: 프로퍼티 쓰기 가로채기**
```javascript
// 서명: set(target, property, value, receiver)
// 반드시 boolean 반환: true(성공), false(실패 → TypeError)
const validated = new Proxy({}, {
    set(target, prop, value) {
        if (prop === 'age') {
            if (typeof value !== 'number' || value < 0 || value > 150) {
                throw new RangeError('유효하지 않은 나이');
            }
        }
        target[prop] = value;
        return true; // 필수
    }
});

validated.age = 25; // ✅
validated.age = -1; // RangeError
```
활용: 입력 유효성 검사, 타입 강제, 변경 감지(반응형 시스템)

**has 트랩: `in` 연산자 가로채기**
```javascript
// 서명: has(target, property)
const range = new Proxy({ min: 1, max: 100 }, {
    has(target, prop) {
        const num = Number(prop);
        return num >= target.min && num <= target.max;
    }
});

50 in range;  // true  (1~100 범위)
150 in range; // false
```
활용: 범위 검사, 숨겨진 프로퍼티 존재 여부 제어

### 꼬리 질문
- `set` 트랩에서 `true`를 반환하지 않으면 어떻게 되나요?
- Proxy 트랩에서 Reflect를 사용해야 하는 이유는 무엇인가요?

### 실무 사례
Vue 3의 반응형 시스템에서 `set` 트랩으로 데이터 변경을 감지하고 의존하는 컴포넌트를 재렌더링합니다.
