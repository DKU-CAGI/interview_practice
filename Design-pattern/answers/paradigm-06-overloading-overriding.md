## Q. 오버로딩과 오버라이딩의 차이는 무엇인가요?

### 핵심 답변
오버로딩(Overloading)은 같은 이름의 메서드를 매개변수를 다르게 하여 여러 개 정의하는 것(컴파일 타임 결정)이고, 오버라이딩(Overriding)은 부모 클래스의 메서드를 자식 클래스에서 재정의하는 것(런타임 결정)입니다.

### 상세 설명

**비교:**
| | 오버로딩 | 오버라이딩 |
|---|---|---|
| 정의 | 같은 이름, 다른 매개변수 | 부모 메서드 재정의 |
| 결정 시점 | 컴파일 타임 | 런타임 |
| 관계 | 같은 클래스 내 | 상속 관계 |
| 목적 | 편의성, 다양한 입력 처리 | 다형성 구현 |

**오버로딩 (Overloading):**
```java
class Calculator {
    // 같은 이름, 다른 매개변수 타입/개수
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
    String add(String a, String b) { return a + b; }
}

Calculator calc = new Calculator();
calc.add(1, 2);       // int 버전 호출
calc.add(1.0, 2.0);   // double 버전 호출
calc.add(1, 2, 3);    // 3인수 버전 호출
```

**오버라이딩 (Overriding):**
```java
class Animal {
    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {
    @Override // 컴파일러에게 오버라이딩 명시 (오타 방지)
    void sound() {
        System.out.println("멍멍"); // 부모 메서드 재정의
    }
}

// 런타임 다형성 (어떤 sound()가 실행될지 런타임에 결정)
Animal animal = new Dog();
animal.sound(); // "멍멍" (Dog의 메서드)
```

**JavaScript에서:**
```javascript
// JS는 정적 타입이 없어 오버로딩을 직접 지원 안 함
// arguments 객체나 기본값으로 유사하게 구현
function add(a, b, c = 0) {
    return a + b + c;
}

// 오버라이딩 (프로토타입 체인)
class Animal {
    sound() { return '동물 소리'; }
}
class Dog extends Animal {
    sound() { return '멍멍'; } // 오버라이딩
}
```

### 꼬리 질문
- 오버라이딩 시 `@Override` 어노테이션을 사용하는 이유는?
- 오버로딩은 다형성인가요?

### 실무 사례
Java의 `toString()`, `equals()`, `hashCode()` 오버라이딩이 가장 흔한 예입니다. Spring의 `WebMvcConfigurer`를 구현하는 것도 오버라이딩입니다.
