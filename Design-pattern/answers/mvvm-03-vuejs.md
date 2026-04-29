## Q. Vue.js가 MVVM 패턴을 어떻게 구현하는지 설명해주세요.

### 핵심 답변
Vue.js는 `data` 옵션이 Model, 템플릿이 View, Vue 인스턴스(Options API) 또는 Composable(Composition API)이 ViewModel 역할을 합니다. Proxy 기반 반응형 시스템으로 데이터 바인딩을 구현합니다.

### 상세 설명

**MVVM 역할 매핑:**
```
Model    → Vue의 data, Pinia store, API 응답 데이터
View     → template (HTML 템플릿)
ViewModel → setup(), data(), computed(), methods() (Vue 인스턴스)
```

**Options API 예시:**
```javascript
// ViewModel
export default {
    name: 'UserList',

    // Model에서 가져온 데이터를 ViewModel에서 관리
    data() {
        return {
            users: [],       // 상태
            isLoading: false, // UI 상태
            searchQuery: '',
        };
    },

    // 파생 상태 (자동 캐싱)
    computed: {
        filteredUsers() {
            return this.users.filter(u =>
                u.name.includes(this.searchQuery)
            );
        }
    },

    // 사용자 액션 처리
    methods: {
        async fetchUsers() {
            this.isLoading = true;
            this.users = await api.getUsers();
            this.isLoading = false;
        }
    },

    // 생명주기
    mounted() {
        this.fetchUsers();
    }
};
```

```html
<!-- View: 템플릿 = 선언적 바인딩 -->
<template>
    <input v-model="searchQuery" placeholder="검색" />
    <!-- v-model = 양방향 바인딩 -->

    <span v-if="isLoading">로딩...</span>

    <ul>
        <li v-for="user in filteredUsers" :key="user.id">
            <!-- {{ }} = 단방향 바인딩 (ViewModel → View) -->
            {{ user.name }} - {{ user.email }}
        </li>
    </ul>
</template>
```

**반응형 데이터 바인딩 원리:**
1. `data()`의 객체를 Proxy로 감싸 변경 감지
2. 템플릿 렌더링 시 data 접근 → 의존성 추적
3. data 변경 → 의존하는 컴포넌트 재렌더링

### 꼬리 질문
- Vue 2와 Vue 3의 반응형 시스템 차이는 무엇인가요?
- Pinia는 MVVM에서 어떤 역할을 하나요?

### 실무 사례
Vue 3 + Pinia 조합에서 Pinia Store가 Model/공유 ViewModel, 컴포넌트의 `setup()`이 로컬 ViewModel 역할을 합니다.
