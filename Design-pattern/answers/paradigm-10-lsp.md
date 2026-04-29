## Q. 리스코프 치환 원칙(LSP)이란 무엇인가요?

### 핵심 답변
LSP는 "자식 클래스는 부모 클래스를 완전히 대체할 수 있어야 한다"는 원칙입니다. 부모 타입의 객체를 자식 타입으로 교체해도 프로그램이 올바르게 동작해야 합니다.

### 상세 설명

**LSP 위반 — 직사각형/정사각형 문제:**
```java
class Rectangle {
    protected int width, height;
    void setWidth(int w) { this.width = w; }
    void setHeight(int h) { this.height = h; }
    int area() { return width * height; }
}

class Square extends Rectangle {
    @Override
    void setWidth(int w) {
        this.width = w;
        this.height = w; // 정사각형은 width = height
    }
    @Override
    void setHeight(int h) {
        this.width = h; // 이 동작이 Rectangle 사용자를 놀라게 함!
        this.height = h;
    }
}

// LSP 위반: Rectangle로 예상한 동작이 Square에서 깨짐
Rectangle rect = new Square();
rect.setWidth(5);
rect.setHeight(10);
System.out.println(rect.area()); // 예상: 50, 실제: 100 (LSP 위반)
```

**LSP 준수:**
```java
// 공통 추상화로 분리
interface Shape {
    int area();
}

class Rectangle implements Shape {
    int width, height;
    public int area() { return width * height; }
}

class Square implements Shape {
    int side;
    public int area() { return side * side; }
}
```

**LSP 준수 여부 확인 방법:**
1. 자식 클래스가 부모의 메서드를 **같은 방식**으로 사용할 수 있는가?
2. 자식 클래스가 부모의 **사전 조건을 강화**하지 않는가?
3. 자식 클래스가 부모의 **사후 조건을 약화**하지 않는가?

**LSP와 다형성:**
```java
// LSP가 지켜져야 다형성이 안전하게 동작
for (Shape shape : shapes) {
    System.out.println(shape.area()); // 어떤 Shape든 올바르게 동작
}
```

### 꼬리 질문
- LSP를 위반하면 어떤 문제가 발생하나요?
- Duck Typing을 사용하는 언어(Python, JavaScript)에서 LSP는 어떻게 적용되나요?

### 실무 사례
Spring의 `JpaRepository`를 구현하면 `findById`, `save` 등의 계약을 지켜야 합니다. 인메모리 테스트 구현체가 실제 JPA 구현체를 대체할 수 있어야 LSP를 지킵니다.
