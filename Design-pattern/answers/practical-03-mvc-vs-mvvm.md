## Q. MVC와 MVVM 중 어떤 패턴을 선택할지 어떻게 결정하나요?

### 핵심 답변
서버 사이드 렌더링, 단순한 UI, 양방향 상호작용이 적은 경우 MVC가 적합하고, 복잡한 UI 상태 관리, 실시간 데이터 반응, SPA 개발에는 MVVM이 더 적합합니다.

### 상세 설명

**선택 기준:**
| 상황 | 추천 패턴 |
|------|---------|
| 서버 사이드 렌더링 (Spring MVC, Django) | MVC |
| 단순한 CRUD 웹앱 | MVC |
| REST API 백엔드 | MVC |
| React/Vue/Angular SPA | MVVM |
| 복잡한 폼과 실시간 유효성 검사 | MVVM |
| 데이터 실시간 반응 필요 | MVVM |
| 테스트 용이성 최우선 | MVVM (ViewModel 독립 테스트) |

**MVC 선택 이유:**
```java
// Spring MVC: 백엔드 API 서버
// - 요청→처리→응답의 단순한 흐름
// - View가 Thymeleaf 템플릿
// - DB CRUD 중심
@RestController
public class ProductController {
    @GetMapping("/products")
    public List<Product> getProducts() {
        return productService.findAll(); // 단순한 흐름
    }
}
```

**MVVM 선택 이유:**
```javascript
// Vue.js: 실시간 검색, 필터, 장바구니 등 복잡한 UI 상태
// - 데이터 변경 → 자동 UI 갱신
// - 양방향 바인딩으로 폼 처리
const { products, searchQuery, filteredProducts, addToCart } = useProductViewModel();
// ViewModel이 UI 상태를 모두 관리
```

**실제 결정 과정:**
1. **UI 복잡도**: 복잡한 상태 → MVVM
2. **렌더링 방식**: SSR → MVC, CSR(SPA) → MVVM
3. **팀 기술 스택**: Java/Spring 팀 → MVC, Vue/React 팀 → MVVM
4. **테스트 전략**: UI 단위 테스트 중요 → MVVM
5. **실시간 데이터**: 빈번한 업데이트 → MVVM

**현대적 풀스택:**
```
백엔드: Spring MVC (REST API)
프론트엔드: Vue.js MVVM (SPA)
→ 두 패턴을 함께 사용하는 것이 일반적
```

### 꼬리 질문
- React는 MVC, MVVM, MVP 중 어디에 가까운가요?
- MVC와 MVVM의 테스트 전략 차이는?

### 실무 사례
대부분의 현대 웹 서비스는 백엔드(Spring MVC/NestJS) + 프론트엔드(React/Vue MVVM)의 조합을 사용합니다. 두 패턴이 상호 배타적이지 않습니다.
