## Q. 팩토리 패턴에서 상위 클래스와 하위 클래스의 역할을 설명해주세요.

### 핵심 답변
상위 클래스(부모)는 객체 생성의 인터페이스와 공통 로직을 정의하고, 하위 클래스(자식)는 실제로 어떤 구체 클래스를 생성할지 결정합니다.

### 상세 설명

**역할 분리의 핵심 원칙:**
- 상위 클래스: **무엇을** 생성할지의 인터페이스 정의, 생성 이후의 공통 처리 담당
- 하위 클래스: **어떤 것을** 생성할지 구체적으로 결정

```javascript
// 상위 클래스: 공통 로직 + 추상 팩토리 메서드
class CoffeeFactory {
    orderCoffee() {
        const coffee = this.createCoffee(); // 하위 클래스에 위임
        coffee.addMilk();
        coffee.addSugar();
        return coffee;
    }

    createCoffee() {
        throw new Error('서브클래스에서 구현 필요');
    }
}

// 하위 클래스: 구체 클래스 결정
class LatteFactory extends CoffeeFactory {
    createCoffee() {
        return new Latte(); // 구체적인 생성 담당
    }
}

class EspressoFactory extends CoffeeFactory {
    createCoffee() {
        return new Espresso();
    }
}

// 클라이언트
const factory = new LatteFactory();
const coffee = factory.orderCoffee(); // Latte 생성 후 공통 처리
```

**Template Method 패턴과의 관계:**
팩토리 메서드 패턴에서 `orderCoffee()`는 Template Method 역할을 합니다. 알고리즘의 골격(공통 처리)은 상위 클래스에, 가변적인 부분(생성 로직)은 하위 클래스에 위임합니다.

**LSP(리스코프 치환 원칙) 적용:**
```javascript
function makeCoffee(factory: CoffeeFactory) {
    return factory.orderCoffee(); // LatteFactory든 EspressoFactory든 동작
}
```

### 꼬리 질문
- 팩토리 메서드 패턴이 Template Method 패턴과 어떻게 관련되나요?
- 새로운 커피 종류를 추가할 때 기존 코드를 얼마나 수정해야 하나요?

### 실무 사례
Java의 `HttpServlet`에서 `service()` 메서드가 상위 클래스 역할을 하고, 개발자가 오버라이드하는 `doGet()`, `doPost()`가 하위 클래스 역할을 합니다.
