## Q. Vue.js 3.0에서 프록시 패턴이 어떻게 활용되는지 설명해주세요.

### 핵심 답변
Vue 3는 `reactive()`와 `ref()`에서 JavaScript `Proxy`를 사용해 데이터 변화를 감지합니다. 데이터에 접근(get)하면 의존성을 수집하고, 변경(set)하면 관련 컴포넌트를 자동으로 재렌더링합니다.

### 상세 설명

**Vue 2 vs Vue 3 반응형 시스템 비교:**
| | Vue 2 | Vue 3 |
|---|---|---|
| 방식 | Object.defineProperty | Proxy |
| 배열 감지 | 7개 메서드만 감지 | 모든 변경 감지 |
| 새 프로퍼티 | `Vue.set()` 필요 | 자동 감지 |
| 성능 | 초기화 시 모든 속성 변환 | 필요 시 변환(Lazy) |

**Vue 3 반응형 구현 원리 (간략화):**
```javascript
function reactive(target) {
    return new Proxy(target, {
        get(target, prop, receiver) {
            // 의존성 추적 (어떤 컴포넌트가 이 데이터에 의존하는지 기록)
            track(target, prop);
            return Reflect.get(target, prop, receiver);
        },
        set(target, prop, value, receiver) {
            const result = Reflect.set(target, prop, value, receiver);
            // 의존하는 컴포넌트들에게 재렌더링 트리거
            trigger(target, prop);
            return result;
        }
    });
}

// 사용
const state = reactive({ count: 0 });

// 컴포넌트에서
effect(() => {
    console.log(state.count); // get → track 실행
});

state.count++; // set → trigger → effect 재실행
```

**Vue 2의 한계 (Object.defineProperty):**
```javascript
// Vue 2에서 이 경우들은 반응형 감지 불가
this.items[0] = 'new'; // 배열 인덱스 직접 변경
this.newProp = 'value'; // 새 프로퍼티 추가
delete this.prop;       // 프로퍼티 삭제
```

### 꼬리 질문
- Vue 3에서 `ref()`와 `reactive()`의 차이는 무엇인가요?
- Vue 3의 Proxy가 배열 변경을 감지하는 방식을 설명해주세요.

### 실무 사례
Vue 3 Composition API에서 `reactive({})`, `ref(0)`, `computed()` 모두 Proxy 기반의 반응형 시스템 위에서 동작합니다.
