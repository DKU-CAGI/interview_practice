## Q. MVVM 패턴이란 무엇이며, MVC와 어떤 차이가 있나요?

### 핵심 답변
MVVM은 Model-View-ViewModel의 약자로, ViewModel이 View와 Model 사이에서 데이터 바인딩으로 동기화를 담당합니다. MVC와 달리 View와 ViewModel이 데이터 바인딩으로 느슨하게 연결되어 View가 ViewModel을 직접 호출하지 않아도 됩니다.

### 상세 설명

**비교:**
| | MVC | MVVM |
|---|---|---|
| 중간 계층 | Controller | ViewModel |
| View 업데이트 방식 | Controller가 직접 업데이트 | 데이터 바인딩으로 자동 동기화 |
| View-로직 결합 | Controller에서 View 참조 | ViewModel은 View를 모름 |
| 양방향 바인딩 | 직접 구현 필요 | 프레임워크 지원 |

**MVC의 흐름:**
```
사용자 입력 → Controller → Model 업데이트
Controller → View.update() 호출 → 화면 갱신 (Controller가 View를 알아야 함)
```

**MVVM의 흐름:**
```
사용자 입력 → ViewModel 업데이트 (또는 양방향 바인딩으로 자동)
ViewModel 상태 변경 → 바인딩으로 View 자동 갱신 (ViewModel은 View를 모름)
```

**Vue.js MVVM:**
```javascript
// ViewModel (Vue 인스턴스)
const vm = new Vue({
    data: { // Model 역할도 겸함
        message: 'Hello',
        count: 0
    },
    methods: {
        increment() { this.count++; } // 비즈니스 로직
    }
});
```
```html
<!-- View: 데이터 바인딩으로 ViewModel과 연결 -->
<div>
    <p>{{ message }}</p>
    <p>Count: {{ count }}</p>
    <button @click="increment">+</button>
</div>
```

**핵심 차이: ViewModel은 View를 모른다**
- MVC Controller: View 인스턴스를 직접 참조하고 업데이트
- MVVM ViewModel: 상태만 관리, View는 바인딩으로 알아서 반응

### 꼬리 질문
- MVVM에서 ViewModel을 단위 테스트하기 쉬운 이유는?
- React의 경우 MVC, MVP, MVVM 중 어떤 패턴에 가깝나요?

### 실무 사례
Vue.js, Angular, WPF, SwiftUI, Jetpack Compose가 MVVM 패턴 기반입니다. React + Redux는 단방향 데이터 흐름으로 약간 다르지만 유사한 구조입니다.
