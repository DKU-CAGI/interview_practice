## Q. 팩토리 패턴이란 무엇이며, 어떤 문제를 해결하나요?

### 핵심 답변
팩토리 패턴은 객체 생성 로직을 별도 클래스(팩토리)로 분리하여, 클라이언트 코드가 구체적인 클래스에 의존하지 않고 객체를 생성할 수 있게 하는 생성 패턴입니다.

### 상세 설명

**해결하는 문제:**
- `new 구체클래스()`가 코드 전반에 퍼지면 구체 클래스에 강하게 결합됨
- 새로운 타입이 추가될 때마다 클라이언트 코드 수정 필요 (OCP 위반)
- 객체 생성 로직의 중복

**팩토리 메서드 패턴:**
```javascript
class CoffeeFactory {
    static createCoffee(type) {
        if (type === 'latte') return new Latte();
        if (type === 'espresso') return new Espresso();
        throw new Error('Unknown coffee type');
    }
}

// 클라이언트는 구체 클래스를 모름
const coffee = CoffeeFactory.createCoffee('latte');
```

**장점:**
- 객체 생성 로직 중앙화 → 변경 시 팩토리만 수정
- 클라이언트와 구체 클래스 간 결합 제거
- OCP 준수: 새 타입 추가 시 클라이언트 코드 변경 불필요

### 장단점
- **장점**: 결합도 감소, 단일 책임 원칙 준수, 확장성 향상
- **단점**: 간단한 생성에는 오버엔지니어링, 클래스 수 증가

### 꼬리 질문
- 팩토리 패턴과 추상 팩토리 패턴의 차이는 무엇인가요?
- 팩토리 패턴이 OCP를 준수하는 방식을 설명해주세요.

### 실무 사례
Java의 `Calendar.getInstance()`, `NumberFormat.getInstance()`, JDBC의 `DriverManager.getConnection()` 등이 팩토리 패턴의 예입니다. React에서도 컴포넌트를 동적으로 렌더링할 때 활용됩니다.
