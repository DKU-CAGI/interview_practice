## Q. 데이터 바인딩(Data Binding)이란 무엇인가요?

### 핵심 답변
데이터 바인딩은 View(UI)와 데이터(Model/ViewModel)를 자동으로 동기화하는 메커니즘입니다. 데이터가 변경되면 View가 자동으로 갱신되고, 사용자 입력이 있으면 데이터도 자동으로 변경됩니다.

### 상세 설명

**바인딩의 종류:**

**1. 단방향 바인딩 (One-Way)**
- 데이터 → View 방향만 동기화
- View가 데이터를 변경해도 반영되지 않음
```html
<!-- Vue: {{ }} 보간법 = 단방향 -->
<p>{{ user.name }}</p>

<!-- Vue: v-bind = 단방향 -->
<img :src="user.avatar" />

<!-- React -->
<p>{user.name}</p>
```

**2. 양방향 바인딩 (Two-Way)**
- 데이터 ↔ View 양방향 동기화
- 사용자 입력이 즉시 데이터에 반영
```html
<!-- Vue: v-model = 양방향 -->
<input v-model="username" />
<!-- username 변경 → input 값 갱신 -->
<!-- input 입력 → username 변경 -->
```

**v-model의 실제 동작 (설탕 문법):**
```html
<!-- 이 두 코드는 동일 -->
<input v-model="username" />

<input
    :value="username"
    @input="username = $event.target.value"
/>
```

**React의 단방향 데이터 흐름:**
```jsx
// React는 기본적으로 단방향
function Input() {
    const [value, setValue] = useState('');

    return (
        <input
            value={value}              // 데이터 → View
            onChange={e => setValue(e.target.value)} // View → 데이터
        />
    );
    // 양방향처럼 보이지만, 명시적 이벤트 핸들러 필요
}
```

### 장단점
- **단방향 장점**: 데이터 흐름 예측 가능, 디버깅 용이
- **양방향 장점**: 폼 처리 간결, 코드량 감소
- **양방향 단점**: 대규모 앱에서 상태 추적 어려움

### 꼬리 질문
- React가 양방향 바인딩 대신 단방향 데이터 흐름을 택한 이유는?
- Vue의 `v-model`을 커스텀 컴포넌트에 적용하는 방법은?

### 실무 사례
폼 입력이 많은 관리자 페이지는 양방향 바인딩(Vue `v-model`)이 편리하고, 복잡한 상태 관리가 필요한 앱은 단방향(React + Redux)이 유리합니다.
