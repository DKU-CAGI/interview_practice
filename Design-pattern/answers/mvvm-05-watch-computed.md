## Q. watch와 computed의 차이점을 설명해주세요.

### 핵심 답변
`computed`는 다른 데이터에서 파생된 읽기 전용 값을 자동으로 캐싱하고, `watch`는 특정 데이터 변화를 감지하여 부수효과(비동기 작업, DOM 조작 등)를 실행합니다.

### 상세 설명

**computed — 파생 상태, 캐싱**
```javascript
const vm = {
    data: {
        firstName: '길동',
        lastName: '홍',
        items: [1, 2, 3, 4, 5],
    },
    computed: {
        fullName() {
            // firstName이나 lastName이 변경될 때만 재계산
            // 그 외에는 캐싱된 값 반환
            return `${this.lastName}${this.firstName}`;
        },
        evenItems() {
            return this.items.filter(n => n % 2 === 0);
        }
    }
};
```

**watch — 부수효과, 비동기 작업**
```javascript
const vm = {
    data: {
        searchQuery: '',
        userId: null,
    },
    watch: {
        // searchQuery 변화 시 API 호출 (부수효과)
        searchQuery(newVal, oldVal) {
            if (newVal !== oldVal) {
                this.search(newVal); // 비동기 작업
            }
        },

        // 깊은 감시
        userId: {
            handler(newId) {
                this.fetchUserProfile(newId); // API 호출
            },
            immediate: true, // 초기에도 실행
        },

        // 깊은 객체 감시
        'user.settings': {
            handler(newSettings) {
                localStorage.setItem('settings', JSON.stringify(newSettings));
            },
            deep: true,
        }
    }
};
```

**언제 무엇을 사용?**
| 상황 | 사용 |
|------|------|
| 여러 데이터에서 파생 값 계산 | `computed` |
| 값을 템플릿에서 표시 | `computed` |
| 데이터 변화 시 API 호출 | `watch` |
| 데이터 변화 시 localStorage 저장 | `watch` |
| 데이터 변화 시 다른 데이터 변경 | `watch` (또는 `computed` setter) |
| 비동기 작업 | `watch` (`computed`는 비동기 불가) |

### 꼬리 질문
- `computed` 속성에 setter를 추가하는 방법과 사용 사례는?
- Vue 3에서 `watchEffect`와 `watch`의 차이는?

### 실무 사례
검색 기능: 검색어를 `watch`로 감시하고 디바운스 후 API 호출. 검색 결과 필터링은 `computed`로 처리. 이 두 가지를 조합하면 성능과 UX를 모두 잡을 수 있습니다.
