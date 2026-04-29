## Q. MVC 패턴이란 무엇이며, 각 구성요소의 역할을 설명해주세요.

### 핵심 답변
MVC는 애플리케이션을 Model(데이터), View(화면), Controller(로직)로 분리하는 아키텍처 패턴입니다. 각 구성요소를 독립적으로 개발·테스트·유지보수할 수 있게 합니다.

### 상세 설명

**각 구성요소의 역할:**

| 구성요소 | 역할 |
|----------|------|
| **Model** | 데이터와 비즈니스 로직 담당. DB 연동, 데이터 유효성 검사 |
| **View** | 사용자에게 데이터 표시. Model 데이터를 기반으로 화면 렌더링 |
| **Controller** | 사용자 입력 처리. Model과 View 사이 조율 |

**흐름:**
```
사용자 입력 → Controller → Model 업데이트
                         → View 갱신 → 사용자에게 표시
```

**코드 예시:**
```javascript
// Model: 데이터와 비즈니스 로직
class UserModel {
    constructor() { this.users = []; }
    addUser(user) { this.users.push(user); }
    getUsers() { return this.users; }
    findById(id) { return this.users.find(u => u.id === id); }
}

// View: 화면 표시
class UserView {
    render(users) {
        return users.map(u => `<li>${u.name}</li>`).join('');
    }
    renderError(message) {
        return `<div class="error">${message}</div>`;
    }
}

// Controller: 조율
class UserController {
    constructor(model, view) {
        this.model = model;
        this.view = view;
    }

    addUser(userData) {
        this.model.addUser(userData);
        const html = this.view.render(this.model.getUsers());
        document.getElementById('list').innerHTML = html;
    }
}
```

**MVC의 핵심 원칙:**
- Model은 View와 Controller를 모름
- View는 Model에서 데이터만 읽음 (가능하면)
- Controller가 Model과 View를 연결

### 장단점
- **장점**: 관심사 분리, 재사용성 향상, 테스트 용이
- **단점**: 작은 프로젝트엔 오버헤드, Controller 비대화 (Massive ViewController)

### 꼬리 질문
- MVC와 MVVM의 차이는 무엇인가요?
- Controller가 비대해지는 문제를 어떻게 해결하나요?

### 실무 사례
Spring MVC, Ruby on Rails, Django, Express.js가 모두 MVC 아키텍처 기반입니다.
