# Design Pattern 면접 질문

## 질문 목록

### 싱글톤 패턴 (Singleton Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 1 | [싱글톤 패턴이란 무엇이며, 어떤 상황에서 사용하나요?](answers/singleton-01-what-is.md) | 하 | |
| 2 | [싱글톤 패턴을 JavaScript에서 구현하는 방법을 설명해주세요.](answers/singleton-02-js-implementation.md) | 중 | |
| 3 | [싱글톤 패턴의 장점과 단점은 무엇인가요?](answers/singleton-03-pros-cons.md) | 하 | |
| 4 | [데이터베이스 연결 시 싱글톤 패턴을 사용하는 이유는 무엇인가요?](answers/singleton-04-db-connection.md) | 중 | |
| 5 | [Java에서 싱글톤 패턴을 구현할 때 'static 키워드'를 사용하는 이유는?](answers/singleton-05-java-static.md) | 중 | |
| 6 | [싱글톤 패턴이 테스트하기 어려운 이유는 무엇인가요?](answers/singleton-06-testing-difficulty.md) | 상 | |

### 팩토리 패턴 (Factory Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 7 | [팩토리 패턴이란 무엇이며, 어떤 문제를 해결하나요?](answers/factory-01-what-is.md) | 하 | |
| 8 | [팩토리 패턴과 추상 팩토리 패턴의 차이는 무엇인가요?](answers/factory-02-vs-abstract-factory.md) | 상 | |
| 9 | [팩토리 패턴에서 상위 클래스와 하위 클래스의 역할을 설명해주세요.](answers/factory-03-superclass-subclass.md) | 중 | |
| 10 | [팩토리 패턴을 사용하면 코드의 유연성이 어떻게 향상되나요?](answers/factory-04-flexibility.md) | 중 | |

### 전략 패턴 (Strategy Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 11 | [전략 패턴이란 무엇이며, 정책 패턴과 어떤 관계가 있나요?](answers/strategy-01-what-is.md) | 하 | |
| 12 | [전략 패턴을 사용하는 실제 사례를 들어주세요 (예: 결제 시스템).](answers/strategy-02-real-examples.md) | 중 | |
| 13 | [passport.js에서 전략 패턴이 어떻게 활용되는지 설명해주세요.](answers/strategy-03-passportjs.md) | 중 | |
| 14 | [전략 패턴을 사용하면 어떤 이점이 있나요?](answers/strategy-04-benefits.md) | 하 | |
| 15 | [컨텍스트(Context)란 무엇이며, 전략 패턴에서 어떤 역할을 하나요?](answers/strategy-05-context.md) | 중 | |
| 16 | [전략 패턴과 상태 패턴의 차이는 무엇인가요?](answers/strategy-06-vs-state.md) | 상 | |

### 옵저버 패턴 (Observer Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 17 | [옵저버 패턴이란 무엇이며, 어떤 상황에서 사용하나요?](answers/observer-01-what-is.md) | 하 | |
| 18 | [옵저버 패턴에서 Subject와 Observer의 역할을 설명해주세요.](answers/observer-02-subject-observer-roles.md) | 중 | |
| 19 | [옵저버 패턴과 MVC 패턴의 관계를 설명해주세요.](answers/observer-03-vs-mvc.md) | 중 | |
| 20 | [트위터의 팔로우 시스템을 옵저버 패턴으로 어떻게 구현할 수 있나요?](answers/observer-04-twitter.md) | 중 | |
| 21 | [register(), unregister(), notifyObservers() 메서드의 역할을 설명해주세요.](answers/observer-05-methods.md) | 중 | |
| 22 | [옵저버 패턴을 사용할 때 발생할 수 있는 문제점은 무엇인가요?](answers/observer-06-problems.md) | 상 | |
| 23 | [이벤트 기반 시스템과 옵저버 패턴의 관계를 설명해주세요.](answers/observer-07-event-driven.md) | 상 | |

### 프록시 패턴 (Proxy Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 24 | [프록시 패턴이란 무엇이며, 어떤 용도로 사용되나요?](answers/proxy-01-what-is.md) | 하 | |
| 25 | [프록시 객체를 사용하는 이점은 무엇인가요?](answers/proxy-02-benefits.md) | 중 | |
| 26 | [JavaScript에서 Proxy 객체를 사용하는 방법을 설명해주세요.](answers/proxy-03-js-proxy.md) | 중 | |
| 27 | [get(), set(), has() 핸들러의 역할을 설명해주세요.](answers/proxy-04-handlers.md) | 중 | |
| 28 | [Vue.js 3.0에서 프록시 패턴이 어떻게 활용되는지 설명해주세요.](answers/proxy-05-vuejs.md) | 중 | |
| 29 | [프록시 서버와 프록시 패턴의 차이점은 무엇인가요?](answers/proxy-06-vs-proxy-server.md) | 하 | |
| 30 | [nginx를 프록시 서버로 사용하는 이유는 무엇인가요?](answers/proxy-07-nginx.md) | 중 | |
| 31 | [CORS 문제를 프록시 서버로 어떻게 해결할 수 있나요?](answers/proxy-08-cors.md) | 중 | |

### 이터레이터 패턴 (Iterator Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 32 | [이터레이터 패턴이란 무엇이며, 어떤 문제를 해결하나요?](answers/iterator-01-what-is.md) | 하 | |
| 33 | [JavaScript에서 for...of 문과 이터레이터의 관계를 설명해주세요.](answers/iterator-02-for-of.md) | 중 | |
| 34 | [Map과 Set 자료구조에서 이터레이터를 어떻게 사용하나요?](answers/iterator-03-map-set.md) | 중 | |
| 35 | [이터레이터 프로토콜과 이터러블 객체의 차이는 무엇인가요?](answers/iterator-04-protocol.md) | 상 | |

### 노출모듈 패턴 (Revealing Module Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 36 | [노출모듈 패턴이란 무엇이며, 왜 사용하나요?](answers/revealing-01-what-is.md) | 하 | |
| 37 | [private과 public 접근 제어를 JavaScript에서 어떻게 구현하나요?](answers/revealing-02-private-public.md) | 중 | |
| 38 | [즉시 실행 함수(IIFE)와 노출모듈 패턴의 관계를 설명해주세요.](answers/revealing-03-iife.md) | 중 | |
| 39 | [CJS(CommonJS) 모듈 방식과 노출모듈 패턴의 관계는?](answers/revealing-04-cjs.md) | 중 | |

### 데코레이터 패턴 (Decorator Pattern)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 40 | [데코레이터 패턴은 어떤 문제를 해결하나요?](answers/decorator-01-what-is.md) | 중 | |

### MVC 패턴

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 41 | [MVC 패턴이란 무엇이며, 각 구성요소의 역할을 설명해주세요.](answers/mvc-01-what-is.md) | 하 | |
| 42 | [스프링(Spring)에서 MVC 패턴이 어떻게 구현되는지 설명해주세요.](answers/mvc-02-spring.md) | 중 | |
| 43 | [@RequestParam, @RequestHeader, @PathVariable의 차이점은?](answers/mvc-03-spring-annotations.md) | 중 | |
| 44 | [MVC 패턴의 장점과 단점은 무엇인가요?](answers/mvc-04-pros-cons.md) | 중 | |

### MVP 패턴

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 45 | [MVP 패턴이란 무엇이며, MVC와 어떤 차이가 있나요?](answers/mvp-01-what-is.md) | 중 | |
| 46 | [Presenter의 역할은 무엇인가요?](answers/mvp-02-presenter.md) | 중 | |
| 47 | [MVP 패턴에서 View와 Presenter의 관계를 설명해주세요.](answers/mvp-03-view-presenter.md) | 중 | |

### MVVM 패턴

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 48 | [MVVM 패턴이란 무엇이며, MVC와 어떤 차이가 있나요?](answers/mvvm-01-what-is.md) | 중 | |
| 49 | [View Model의 역할은 무엇인가요?](answers/mvvm-02-viewmodel.md) | 중 | |
| 50 | [Vue.js가 MVVM 패턴을 어떻게 구현하는지 설명해주세요.](answers/mvvm-03-vuejs.md) | 중 | |
| 51 | [데이터 바인딩(Data Binding)이란 무엇인가요?](answers/mvvm-04-data-binding.md) | 하 | |
| 52 | [watch와 computed의 차이점을 설명해주세요.](answers/mvvm-05-watch-computed.md) | 중 | |
| 53 | [양방향 데이터 바인딩의 장단점은 무엇인가요?](answers/mvvm-06-two-way-binding.md) | 중 | |

### 의존성 주입 (Dependency Injection)

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 54 | [의존성 주입이란 무엇이며, 왜 필요한가요?](answers/di-01-what-is.md) | 중 | |
| 55 | [의존성 역전 원칙과 의존성 주입의 관계를 설명해주세요.](answers/di-02-dip-relation.md) | 상 | |
| 56 | [의존성 주입 컨테이너(Dependency Injector)의 역할은?](answers/di-03-container.md) | 중 | |
| 57 | [메인 모듈과 하위 모듈 간의 의존성을 어떻게 관리하나요?](answers/di-04-module-management.md) | 상 | |

### 디자인 패턴 일반

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 58 | [디자인 패턴이란 무엇이며, 왜 사용하나요?](answers/general-01-what-is.md) | 하 | |
| 59 | [라이브러리와 프레임워크에서 디자인 패턴이 어떻게 사용되나요?](answers/general-02-library-framework.md) | 중 | |
| 60 | [여러 디자인 패턴을 조합해서 사용할 수 있나요? 예를 들어주세요.](answers/general-03-combining-patterns.md) | 상 | |
| 61 | [디자인 패턴을 너무 많이 사용하면 어떤 문제가 발생할 수 있나요?](answers/general-04-overuse.md) | 중 | |
| 62 | [TDD(Test Driven Development)와 디자인 패턴의 관계는?](answers/general-05-tdd.md) | 상 | |

### 프로그래밍 패러다임

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 63 | [선언형 프로그래밍과 함수형 프로그래밍의 차이는?](answers/paradigm-01-declarative-functional.md) | 중 | |
| 64 | [순수 함수란 무엇이며, 어떤 특징이 있나요?](answers/paradigm-02-pure-function.md) | 중 | |
| 65 | [고차 함수란 무엇인가요?](answers/paradigm-03-higher-order.md) | 중 | |
| 66 | [일급 객체의 조건은 무엇인가요?](answers/paradigm-04-first-class.md) | 중 | |
| 67 | [객체지향 프로그래밍의 4가지 특징을 설명해주세요 (추상화, 캡슐화, 상속성, 다형성).](answers/paradigm-05-oop-4.md) | 하 | |
| 68 | [오버로딩과 오버라이딩의 차이는 무엇인가요?](answers/paradigm-06-overloading-overriding.md) | 하 | |
| 69 | [SOLID 원칙에 대해 설명해주세요.](answers/paradigm-07-solid.md) | 중 | |
| 70 | [단일 책임 원칙(SRP)이란 무엇인가요?](answers/paradigm-08-srp.md) | 중 | |
| 71 | [개방-폐쇄 원칙(OCP)이란 무엇인가요?](answers/paradigm-09-ocp.md) | 중 | |
| 72 | [리스코프 치환 원칙(LSP)이란 무엇인가요?](answers/paradigm-10-lsp.md) | 상 | |
| 73 | [인터페이스 분리 원칙(ISP)이란 무엇인가요?](answers/paradigm-11-isp.md) | 중 | |
| 74 | [의존 역전 원칙(DIP)이란 무엇인가요?](answers/paradigm-12-dip.md) | 중 | |
| 75 | [절차형 프로그래밍과 객체지향 프로그래밍의 차이는?](answers/paradigm-13-procedural-vs-oop.md) | 하 | |

### 실무 적용

| # | 질문 | 난이도 | 상태 |
|---|------|--------|------|
| 76 | [싱글톤 패턴을 남용하면 어떤 문제가 발생하나요?](answers/practical-01-singleton-abuse.md) | 상 | |
| 77 | [프록시 서버를 사용하지 않고 CORS 문제를 해결할 수 있나요?](answers/practical-02-cors-without-proxy.md) | 중 | |
| 78 | [MVC와 MVVM 중 어떤 패턴을 선택할지 어떻게 결정하나요?](answers/practical-03-mvc-vs-mvvm.md) | 상 | |
| 79 | [MongoDB에서 싱글톤 패턴을 사용하는 이유는?](answers/practical-04-mongodb-singleton.md) | 중 | |
| 80 | [프록시 서버로 nginx와 CloudFlare 중 무엇을 선택할지 어떻게 결정하나요?](answers/practical-05-nginx-vs-cloudflare.md) | 중 | |

---

## 통계

| 분류 | 질문 수 |
|------|--------|
| 싱글톤 패턴 | 6 |
| 팩토리 패턴 | 4 |
| 전략 패턴 | 6 |
| 옵저버 패턴 | 7 |
| 프록시 패턴 | 8 |
| 이터레이터 패턴 | 4 |
| 노출모듈 패턴 | 4 |
| 데코레이터 패턴 | 1 |
| MVC 패턴 | 4 |
| MVP 패턴 | 3 |
| MVVM 패턴 | 6 |
| 의존성 주입 | 4 |
| 디자인 패턴 일반 | 5 |
| 프로그래밍 패러다임 | 13 |
| 실무 적용 | 5 |
| **합계** | **80** |

---

## 답변 파일 작성 가이드

```markdown
## Q. {질문 내용}

### 핵심 답변
{1-3문장으로 핵심만 요약}

### 상세 설명
{개념 설명, 구조, 동작 방식 등}

### 코드 예시
\`\`\`javascript
// 예시 코드
\`\`\`

### 장단점
- **장점**: 
- **단점**: 

### 꼬리 질문
- {예상 꼬리 질문 1}
- {예상 꼬리 질문 2}

### 실무 사례
{실제 프레임워크나 라이브러리에서의 사용 예}
```
