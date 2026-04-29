## Q. 팩토리 패턴과 추상 팩토리 패턴의 차이는 무엇인가요?

### 핵심 답변
팩토리 메서드 패턴은 한 종류의 객체를 생성하는 인터페이스를 정의하고, 추상 팩토리 패턴은 관련 있는 여러 종류의 객체군(family)을 함께 생성하는 인터페이스를 정의합니다.

### 상세 설명

**팩토리 메서드 패턴:**
- 하나의 제품 생성에 집중
- 서브클래스에서 어떤 클래스를 인스턴스화할지 결정

```java
// 추상 팩토리 메서드
abstract class CoffeeFactory {
    abstract Coffee createCoffee();

    void prepare() {
        Coffee coffee = createCoffee(); // 서브클래스가 결정
        coffee.brew();
    }
}

class LatteFactory extends CoffeeFactory {
    @Override
    Coffee createCoffee() { return new Latte(); }
}
```

**추상 팩토리 패턴:**
- 관련된 객체군을 함께 생성
- 특정 "테마" 또는 "플랫폼"에 맞는 제품군 일관성 보장

```java
// UI 컴포넌트 팩토리 예시
interface UIFactory {
    Button createButton();
    TextField createTextField();
    Checkbox createCheckbox();
}

class WindowsUIFactory implements UIFactory {
    public Button createButton() { return new WindowsButton(); }
    public TextField createTextField() { return new WindowsTextField(); }
    public Checkbox createCheckbox() { return new WindowsCheckbox(); }
}

class MacUIFactory implements UIFactory {
    public Button createButton() { return new MacButton(); }
    public TextField createTextField() { return new MacTextField(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}
```

**비교 표:**
| | 팩토리 메서드 | 추상 팩토리 |
|---|---|---|
| 생성 단위 | 단일 제품 | 제품군(family) |
| 확장 방법 | 서브클래스 오버라이드 | 새 팩토리 클래스 추가 |
| 목적 | 생성 클래스 위임 | 관련 제품군 일관성 |

### 꼬리 질문
- 추상 팩토리 패턴을 사용하는 실제 사례를 들어주세요.
- 팩토리 패턴과 빌더(Builder) 패턴의 차이는?

### 실무 사례
Java Swing/AWT의 LookAndFeel, 크로스플랫폼 UI 프레임워크에서 추상 팩토리를 사용합니다. Spring의 `ApplicationContext`도 다양한 빈 생성을 추상 팩토리처럼 관리합니다.
