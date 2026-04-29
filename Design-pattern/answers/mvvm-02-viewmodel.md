## Q. View Model의 역할은 무엇인가요?

### 핵심 답변
ViewModel은 View에 표시할 데이터와 상태를 관리하고, 사용자 액션을 처리하며, View와는 데이터 바인딩으로만 통신합니다. View를 직접 참조하지 않아 테스트가 용이합니다.

### 상세 설명

**ViewModel의 구체적 책임:**
1. **View 상태 관리**: 로딩 여부, 에러 메시지, 표시 데이터
2. **사용자 인터랙션 처리**: 버튼 클릭, 입력 이벤트 처리
3. **Model과의 통신**: 데이터 조회/저장
4. **데이터 변환**: Model 데이터를 View에 맞게 포맷팅

```javascript
// Vue 3 Composition API (ViewModel 역할)
import { ref, computed } from 'vue';
import { userApi } from './api';

export function useUserViewModel() {
    // View 상태
    const users = ref([]);
    const isLoading = ref(false);
    const error = ref(null);
    const searchQuery = ref('');

    // 파생 상태 (computed)
    const filteredUsers = computed(() =>
        users.value.filter(u =>
            u.name.includes(searchQuery.value)
        )
    );

    // 사용자 액션 처리
    async function loadUsers() {
        isLoading.value = true;
        error.value = null;
        try {
            users.value = await userApi.getAll();
        } catch (e) {
            error.value = '사용자 목록을 불러올 수 없습니다.';
        } finally {
            isLoading.value = false;
        }
    }

    async function deleteUser(id) {
        await userApi.delete(id);
        users.value = users.value.filter(u => u.id !== id);
    }

    // View에 노출할 것들 반환
    return { users, isLoading, error, filteredUsers, searchQuery, loadUsers, deleteUser };
}
```

```html
<!-- View: ViewModel의 상태를 바인딩 -->
<template>
    <div v-if="isLoading">로딩 중...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <ul v-else>
        <li v-for="user in filteredUsers" :key="user.id">
            {{ user.name }}
            <button @click="deleteUser(user.id)">삭제</button>
        </li>
    </ul>
</template>
```

**ViewModel이 View를 모르는 이유:**
- View를 참조하면 Android/iOS/Web마다 다른 ViewModel이 필요
- View 없이 ViewModel 단위 테스트 가능

### 꼬리 질문
- Android의 Jetpack ViewModel과 MVVM의 ViewModel이 동일한가요?
- ViewModel에 비즈니스 로직이 너무 많아지면 어떻게 해결하나요?

### 실무 사례
Android Jetpack ViewModel은 Activity/Fragment 생명주기에 독립적으로 데이터를 유지하며, LiveData/StateFlow로 View와 바인딩합니다.
