## Q. MVP 패턴에서 View와 Presenter의 관계를 설명해주세요.

### 핵심 답변
View와 Presenter는 인터페이스를 통해 양방향으로 통신합니다. View는 사용자 입력을 Presenter에 위임하고, Presenter는 View 인터페이스를 통해 UI를 제어합니다. 둘은 인터페이스로만 알고 있어 테스트와 교체가 용이합니다.

### 상세 설명

**양방향 통신 구조:**
```
View → Presenter: 사용자 이벤트 전달 (버튼 클릭, 텍스트 입력 등)
Presenter → View: UI 업데이트 명령 (showData, showError, navigate 등)
```

**인터페이스 기반 통신의 이점:**
```java
// View 인터페이스: Presenter가 View를 이 계약으로만 알면 됨
public interface ProductListView {
    void showProducts(List<ProductDto> products);
    void showEmptyState();
    void showError(String message);
    void showLoading(boolean show);
    void navigateToDetail(Long productId);
}

// Presenter 인터페이스: View가 Presenter를 이 계약으로만 알면 됨
public interface ProductListPresenter {
    void loadProducts();
    void onProductClicked(Long productId);
    void onSearchQuery(String query);
    void onDestroy(); // 리소스 정리
}
```

**구현 분리:**
```java
// 실제 View (Android Fragment)
public class ProductListFragment
        extends Fragment implements ProductListView {

    private ProductListPresenter presenter;

    @Override
    public void onViewCreated(...) {
        presenter = new ProductListPresenterImpl(this, productRepository);
        presenter.loadProducts();
    }

    @Override
    public void showProducts(List<ProductDto> products) {
        adapter.setProducts(products); // UI만 처리
    }
}

// 실제 Presenter
public class ProductListPresenterImpl implements ProductListPresenter {
    private final ProductListView view;
    private final ProductRepository repository;

    @Override
    public void loadProducts() {
        view.showLoading(true);
        repository.getAll()
            .subscribe(products -> {
                view.showLoading(false);
                if (products.isEmpty()) view.showEmptyState();
                else view.showProducts(products);
            });
    }
}
```

**테스트 시 Mock View:**
```java
@Test
public void 상품없을때_빈상태_표시() {
    ProductListView mockView = mock(ProductListView.class);
    when(mockRepo.getAll()).thenReturn(Observable.just(emptyList()));

    presenter = new ProductListPresenterImpl(mockView, mockRepo);
    presenter.loadProducts();

    verify(mockView).showEmptyState();
    verify(mockView, never()).showProducts(any());
}
```

### 꼬리 질문
- Presenter에서 Android Context를 참조하면 안 되는 이유는?
- View가 Presenter를 참조하는 것과 Presenter가 View를 참조하는 것의 차이는?

### 실무 사례
MVP는 Angular의 컴포넌트 + 서비스 패턴과 유사합니다. 컴포넌트(View)는 UI만, 서비스(Presenter 역할)가 로직을 담당합니다.
