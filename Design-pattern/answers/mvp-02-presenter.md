## Q. Presenter의 역할은 무엇인가요?

### 핵심 답변
Presenter는 MVC의 Controller를 더 강화한 역할로, View의 모든 UI 로직과 Model과의 모든 상호작용을 담당합니다. View는 수동적으로 Presenter의 명령을 실행하기만 합니다.

### 상세 설명

**Presenter의 구체적 책임:**
1. **사용자 이벤트 처리**: View에서 이벤트를 받아 처리
2. **Model 조작**: 데이터 조회/저장/수정/삭제
3. **데이터 변환**: Model 데이터를 View에 맞게 포맷팅
4. **View 제어**: 어떤 화면 요소를 표시/숨길지 결정

```java
public class LoginPresenter {
    private final LoginView view;
    private final AuthModel authModel;
    private final UserModel userModel;

    // 사용자가 로그인 버튼 클릭 시 View가 이 메서드 호출
    public void onLoginButtonClicked(String email, String password) {
        // 1. 입력 유효성 검사
        if (email.isEmpty() || password.isEmpty()) {
            view.showError("이메일과 비밀번호를 입력하세요.");
            return;
        }

        // 2. 로딩 UI 제어
        view.showLoading();
        view.setLoginButtonEnabled(false);

        // 3. Model 호출 (비즈니스 로직)
        authModel.login(email, password)
            .subscribe(
                token -> {
                    // 4. 성공: View 제어
                    view.hideLoading();
                    view.navigateToMain();
                },
                error -> {
                    // 5. 실패: View에 에러 표시
                    view.hideLoading();
                    view.setLoginButtonEnabled(true);
                    view.showError("로그인 실패: " + error.getMessage());
                }
            );
    }
}
```

**MVC Controller와의 차이:**
```
Controller: HTTP 요청을 라우팅하는 역할이 강함
Presenter: View와 1:1 관계, View UI 로직 전부 담당
```

**테스트 장점:**
```java
// Presenter는 Android 없이 순수 JUnit 테스트 가능
@Test
public void 이메일_비어있으면_에러표시() {
    LoginPresenter presenter = new LoginPresenter(mockView, mockAuth);
    presenter.onLoginButtonClicked("", "password");
    verify(mockView).showError("이메일과 비밀번호를 입력하세요.");
}
```

### 꼬리 질문
- Presenter가 비대해지는 문제를 어떻게 해결하나요?
- Presenter에서 비동기 작업을 어떻게 처리해야 하나요?

### 실무 사례
MVP에서 Presenter 비대화 방지를 위해 UseCase/Interactor 레이어를 추가하는 Clean Architecture와 조합하여 사용합니다.
