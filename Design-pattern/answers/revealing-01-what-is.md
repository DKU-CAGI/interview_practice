## Q. 노출모듈 패턴이란 무엇이며, 왜 사용하나요?

### 핵심 답변
노출모듈 패턴(Revealing Module Pattern)은 모든 로직을 private으로 유지하고, 외부에 공개할 것만 선택적으로 반환하여 명확한 public/private 인터페이스를 정의하는 패턴입니다.

### 상세 설명

**왜 사용하나요?**
- JavaScript(ES6 이전)에는 클래스의 `private` 키워드가 없어 캡슐화 구현 어려움
- 전역 변수 오염 방지
- 모듈 인터페이스를 명확하게 문서화

**기본 구조:**
```javascript
const counterModule = (() => {
    // private 변수/함수 (외부 접근 불가)
    let count = 0;
    const MAX = 100;

    function validate(value) {
        return value >= 0 && value <= MAX;
    }

    function increment() {
        if (validate(count + 1)) count++;
    }

    function decrement() {
        if (validate(count - 1)) count--;
    }

    function reset() {
        count = 0;
    }

    function getCount() {
        return count;
    }

    // public으로 노출할 것만 반환
    return {
        increment,
        decrement,
        reset,
        getCount,
    };
    // count, MAX, validate는 반환하지 않아 private 유지
})();

counterModule.increment();
counterModule.increment();
console.log(counterModule.getCount()); // 2
console.log(counterModule.count); // undefined (private)
```

### 장단점
- **장점**: 캡슐화, 전역 변수 오염 방지, 명확한 인터페이스, 재정의 방지
- **단점**: private 메서드가 public 메서드를 참조할 때 패치(override)가 어려움, 테스트하기 어려운 private 로직

### 꼬리 질문
- 노출모듈 패턴이 클로저를 활용하는 방식을 설명해주세요.
- ES6 모듈(`import`/`export`)이 등장한 후 노출모듈 패턴의 위상은?

### 실무 사례
jQuery의 내부 구현, 많은 레거시 라이브러리가 이 패턴을 사용합니다. 현재는 ES6 모듈이나 TypeScript의 `private` 키워드로 대체되고 있습니다.
