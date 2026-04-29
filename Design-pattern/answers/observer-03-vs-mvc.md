## Q. 옵저버 패턴과 MVC 패턴의 관계를 설명해주세요.

### 핵심 답변
MVC 패턴에서 Model-View 간의 동기화가 옵저버 패턴으로 구현됩니다. Model이 Subject 역할을 하고 View가 Observer 역할을 하여, Model 데이터 변경 시 View가 자동으로 갱신됩니다.

### 상세 설명

**MVC에서 옵저버 패턴 적용:**
```
Model (Subject) → 데이터 변경 → notifyObservers()
View (Observer) → update() 수신 → 화면 갱신
```

```javascript
// Model = Subject
class UserModel {
    constructor() {
        this.observers = [];
        this.user = null;
    }

    register(view) { this.observers.push(view); }

    setUser(user) {
        this.user = user;
        this.observers.forEach(v => v.update(this.user)); // View에 알림
    }
}

// View = Observer
class UserProfileView {
    update(user) {
        document.getElementById('username').textContent = user.name;
        document.getElementById('email').textContent = user.email;
    }
}

class UserSidebarView {
    update(user) {
        document.getElementById('sidebar-name').textContent = user.name;
    }
}

// Controller가 조합
const model = new UserModel();
model.register(new UserProfileView());
model.register(new UserSidebarView());

// Controller
model.setUser({ name: '홍길동', email: 'hong@example.com' });
// → ProfileView, SidebarView 모두 자동 갱신
```

**핵심 이점:**
- Model과 View의 **느슨한 결합**: Model은 View의 구체 타입을 모름
- **하나의 Model, 여러 View**: 동일 데이터를 다양한 형태로 표시
- View가 추가되어도 Model 수정 불필요 (OCP 준수)

### 꼬리 질문
- MVC에서 Controller는 옵저버 패턴에서 어떤 역할을 하나요?
- MVVM의 데이터 바인딩은 옵저버 패턴과 어떻게 다른가요?

### 실무 사례
React의 상태 관리 라이브러리 Redux에서 `store`가 Subject, 연결된 컴포넌트들이 Observer 역할을 합니다. `useSelector`는 store 변경 구독을 추상화한 것입니다.
