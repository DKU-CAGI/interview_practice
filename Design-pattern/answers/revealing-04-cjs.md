## Q. CJS(CommonJS) 모듈 방식과 노출모듈 패턴의 관계는?

### 핵심 답변
CommonJS는 노출모듈 패턴의 아이디어를 Node.js의 공식 모듈 시스템으로 형식화한 것입니다. 노출모듈 패턴이 IIFE + return으로 구현하는 것을 `module.exports` + `require()`로 표준화했습니다.

### 상세 설명

**개념 비교:**
| | 노출모듈 패턴 (IIFE) | CommonJS |
|---|---|---|
| private | 함수 스코프 내부 | 파일/모듈 스코프 내부 |
| public 노출 | `return { ... }` | `module.exports = { ... }` |
| 가져오기 | 전역 변수 | `require('./module')` |
| 캐싱 | 없음 | 자동 캐싱 (싱글톤) |

**같은 목적, 다른 구현:**
```javascript
// 노출모듈 패턴
const math = (() => {
    function square(n) { return n * n; }
    function cube(n) { return n * n * n; }
    const PI = 3.14159;

    return { square, cube, PI }; // public만 반환
})();

// CommonJS (math.js)
const PI = 3.14159; // 파일 스코프 = private
function square(n) { return n * n; }
function cube(n) { return n * n * n; }

module.exports = { square, cube, PI }; // public만 내보냄

// 사용
const math = require('./math');
console.log(math.square(3)); // 9
```

**CommonJS의 추가 이점:**
- 파일 단위 모듈 분리 (코드 조직화)
- `require()` 결과 자동 캐싱 → 싱글톤 효과
- 순환 의존성 처리
- 동기 로딩 (서버 환경에 적합)

**ES Module(ESM) vs CommonJS:**
```javascript
// ESM (브라우저/Node.js 최신)
export const PI = 3.14159;
export function square(n) { return n * n; }

import { square, PI } from './math.js';
```

### 꼬리 질문
- CommonJS와 ES Module의 주요 차이점은 무엇인가요?
- Node.js에서 CommonJS 캐싱이 싱글톤처럼 동작하는 이유는?

### 실무 사례
현재 Node.js 생태계는 CommonJS에서 ES Module로 전환 중입니다. `package.json`의 `"type": "module"` 설정으로 ESM을 활성화합니다.
