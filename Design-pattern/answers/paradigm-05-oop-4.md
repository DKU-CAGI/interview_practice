## Q. 객체지향 프로그래밍의 4가지 특징을 설명해주세요 (추상화, 캡슐화, 상속성, 다형성).

### 핵심 답변
OOP의 4대 특징: **추상화**(필요한 것만 노출), **캡슐화**(데이터와 메서드 묶음+접근 제어), **상속**(부모 속성/메서드 재사용), **다형성**(같은 인터페이스로 다른 동작)입니다.

### 상세 설명

**1. 추상화 (Abstraction)**
> 복잡한 내부를 숨기고 핵심만 노출

```java
// Car 사용자는 내부 엔진 작동을 알 필요 없음
abstract class Car {
    abstract void start(); // 인터페이스만 노출
    abstract void stop();
}
// 내부 구현(엔진 제어, 연료 분사)은 숨김
```

**2. 캡슐화 (Encapsulation)**
> 데이터와 이를 조작하는 메서드를 하나로 묶고, 접근 제어

```java
class BankAccount {
    private double balance; // 직접 접근 차단

    public void deposit(double amount) {
        if (amount > 0) balance += amount; // 유효성 검사 후 변경
    }

    public double getBalance() { return balance; } // 읽기만 허용
}
```

**3. 상속 (Inheritance)**
> 부모 클래스의 속성/메서드를 자식 클래스가 재사용

```java
class Animal {
    String name;
    void eat() { System.out.println(name + " 먹는 중"); }
}

class Dog extends Animal {
    void bark() { System.out.println("멍멍"); }
}

Dog dog = new Dog();
dog.eat(); // 부모 메서드 재사용
dog.bark(); // 자식 고유 메서드
```

**4. 다형성 (Polymorphism)**
> 같은 인터페이스/메서드가 타입에 따라 다르게 동작

```java
// 오버라이딩 (런타임 다형성)
class Animal { void sound() { System.out.println("..."); } }
class Dog extends Animal { void sound() { System.out.println("멍멍"); } }
class Cat extends Animal { void sound() { System.out.println("야옹"); } }

Animal[] animals = { new Dog(), new Cat() };
for (Animal a : animals) {
    a.sound(); // 실제 타입에 따라 다른 동작
}

// 오버로딩 (컴파일 타임 다형성)
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; } // 같은 이름, 다른 타입
}
```

### 꼬리 질문
- 상속 대신 컴포지션(Composition)을 선호하는 이유는?
- 다형성이 OCP(개방-폐쇄 원칙)와 어떻게 연관되나요?

### 실무 사례
Java Spring: `@Service`, `@Repository` 인터페이스 + 구현체로 추상화와 다형성을 활용합니다. 테스트 시 Mock 구현체를 주입하는 것이 다형성의 실용적 활용입니다.
