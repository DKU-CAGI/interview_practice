## Q. 라이브러리와 프레임워크에서 디자인 패턴이 어떻게 사용되나요?

### 핵심 답변
대부분의 프레임워크와 라이브러리는 여러 디자인 패턴을 조합하여 구현됩니다. 개발자가 프레임워크를 이해하고 효과적으로 사용하려면 내재된 패턴을 이해하는 것이 중요합니다.

### 상세 설명

**Spring Framework에서의 패턴:**
| 패턴 | Spring에서의 활용 |
|------|-----------------|
| 싱글톤 | 기본 Bean 스코프 |
| 팩토리 | `BeanFactory`, `ApplicationContext` |
| 프록시 | AOP (`@Transactional`, `@Cacheable`) |
| 옵저버 | `ApplicationEvent`, `@EventListener` |
| 템플릿 메서드 | `JdbcTemplate`, `RestTemplate` |
| 전략 | `AuthenticationStrategy` |
| 데코레이터 | `HttpServletRequestWrapper` |

**React에서의 패턴:**
```javascript
// 컴포지트 패턴: 컴포넌트 트리
<App>
    <Header />
    <Main>
        <Sidebar />
        <Content />
    </Main>
</App>

// 옵저버 패턴: Redux store.subscribe()
const unsubscribe = store.subscribe(() => {
    console.log(store.getState());
});

// 팩토리 패턴: createElement
React.createElement('div', { className: 'container' }, children)

// 고차 컴포넌트 = 데코레이터 패턴
const withAuth = (Component) => (props) => {
    if (!isAuthenticated()) return <Redirect to="/login" />;
    return <Component {...props} />;
};
```

**Node.js에서의 패턴:**
```javascript
// 옵저버: EventEmitter
const emitter = new EventEmitter();
emitter.on('data', handler); // register
emitter.emit('data', payload); // notify

// 미들웨어 = 체인 오브 리스폰서빌리티
app.use(logger);
app.use(authenticate);
app.use(rateLimiter);
```

### 꼬리 질문
- 미들웨어 패턴은 어떤 디자인 패턴과 관련이 있나요?
- React의 Higher-Order Component가 데코레이터 패턴인 이유는?

### 실무 사례
Express.js의 미들웨어 체인, Spring의 Filter Chain, Django의 Middleware가 모두 책임 연쇄(Chain of Responsibility) 패턴입니다.
