## Q. 즉시 실행 함수(IIFE)와 노출모듈 패턴의 관계를 설명해주세요.

### 핵심 답변
노출모듈 패턴은 IIFE(Immediately Invoked Function Expression)를 핵심 메커니즘으로 사용합니다. IIFE가 독립된 스코프를 만들어 private 변수를 보호하고, 반환 객체를 통해 public 인터페이스를 노출합니다.

### 상세 설명

**IIFE란:**
```javascript
// 즉시 실행 함수: 선언과 동시에 실행, 독립된 스코프 생성
(function() {
    const secret = '비밀'; // 함수 스코프 내에서만 존재
    console.log('실행됨');
})();

// 화살표 함수 버전
(() => {
    const secret = '비밀';
})();
```

**IIFE의 역할:**
1. 독립된 함수 스코프 생성 → 전역 변수 오염 방지
2. 함수 실행 후 스코프 소멸 (클로저 제외)
3. 내부 변수가 외부에서 접근 불가 → private 효과

**노출모듈 패턴에서의 활용:**
```javascript
const logger = (() => {
    // IIFE 내부 = private 스코프
    const logs = [];
    const MAX_LOGS = 1000;

    function addLog(level, message) {
        if (logs.length >= MAX_LOGS) logs.shift(); // 오래된 로그 제거
        logs.push({ level, message, time: new Date() });
    }

    function formatLog(log) {
        return `[${log.level}] ${log.time.toISOString()}: ${log.message}`;
    }

    // public 인터페이스만 반환
    return {
        info(msg) { addLog('INFO', msg); },
        error(msg) { addLog('ERROR', msg); },
        getLogs() { return [...logs]; }, // 복사본 반환 (불변성)
        print() { logs.forEach(l => console.log(formatLog(l))); },
    };
})(); // 즉시 실행

logger.info('앱 시작');
logger.error('오류 발생');
logger.print();
// logs, MAX_LOGS, addLog, formatLog는 외부 접근 불가
```

**왜 IIFE를 썼나?**
ES6 `import`/`export`가 없던 시절, 파일 단위 모듈이 없었기 때문에 IIFE로 모듈화 효과를 달성했습니다.

### 꼬리 질문
- IIFE와 블록 스코프(`{}`)의 차이는 무엇인가요?
- `(function(){})()` vs `(function(){}())` — 두 형태의 차이는?

### 실무 사례
jQuery의 전체 코드가 `(function(global, factory){...})(this, ...)` 형태의 IIFE로 감싸져 있습니다.
