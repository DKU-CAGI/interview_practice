## Q. MVP 패턴이란 무엇이며, MVC와 어떤 차이가 있나요?

### 핵심 답변
MVP는 MVC에서 Controller를 Presenter로 교체한 패턴입니다. View와 Model이 완전히 분리되고, Presenter가 둘 사이의 모든 상호작용을 담당합니다. View는 수동적(Passive)으로 Presenter의 명령만 실행합니다.

### 상세 설명

**MVC vs MVP 핵심 차이:**
| | MVC | MVP |
|---|---|---|
| View-Model 관계 | View가 Model 직접 참조 가능 | View는 Model을 모름 |
| 중간 매개체 | Controller (느슨한 역할) | Presenter (모든 로직 담당) |
| View 역할 | 능동적 | 수동적(Passive View) |
| 테스트 | Controller 단위 테스트 | Presenter 단위 테스트 용이 |

**MVP 구조:**
```
사용자 입력 → View → Presenter → Model
             View ← Presenter ← Model
(View와 Model은 직접 통신하지 않음)
```

**코드 예시:**
```java
// View 인터페이스 (Presenter가 View를 이 인터페이스로만 알면 됨)
public interface UserView {
    void showUser(UserDto user);
    void showError(String message);
    void showLoading();
    void hideLoading();
}

// Presenter: 비즈니스 로직 + View/Model 조율
public class UserPresenter {
    private final UserView view;
    private final UserModel model;

    public UserPresenter(UserView view, UserModel model) {
        this.view = view;
        this.model = model;
    }

    public void loadUser(Long id) {
        view.showLoading();
        try {
            UserDto user = model.findById(id);
            view.showUser(user);   // Model → View 전달
        } catch (Exception e) {
            view.showError(e.getMessage());
        } finally {
            view.hideLoading();
        }
    }
}

// View 구현 (Android Activity 예시)
public class UserActivity implements UserView {
    private UserPresenter presenter;

    @Override
    public void showUser(UserDto user) {
        nameTextView.setText(user.getName()); // UI 업데이트만
    }
}
```

### 꼬리 질문
- MVP에서 View 인터페이스를 사용하는 이유는 무엇인가요?
- MVP와 MVVM을 언제 선택해야 하나요?

### 실무 사례
Android 개발에서 MVP가 많이 사용됩니다. Activity/Fragment가 View를 구현하고, Presenter는 순수 Java/Kotlin 클래스로 작성하여 단위 테스트가 용이합니다.
