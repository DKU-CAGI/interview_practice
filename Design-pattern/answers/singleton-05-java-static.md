## Q. Java에서 싱글톤 패턴을 구현할 때 'static 키워드'를 사용하는 이유는?

### 핵심 답변
static을 사용하면 인스턴스 없이도 접근 가능한 클래스 수준의 변수/메서드를 만들 수 있어, 싱글톤 인스턴스를 저장하고 반환하는 데 필수적입니다.

### 상세 설명

**static의 역할:**
- `static 변수`: 클래스 로딩 시 JVM 메서드 영역(Method Area)에 하나만 할당
- `static 메서드`: 인스턴스 없이 `클래스명.메서드()` 형태로 호출 가능

```java
// 기본 구현 (Thread-unsafe)
public class Singleton {
    private static Singleton instance; // 클래스 수준에서 하나

    private Singleton() {} // 외부 new 불가

    public static Singleton getInstance() { // 인스턴스 없이 호출 가능
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Thread-safe 구현 (Double-Checked Locking):**
```java
public class Singleton {
    private static volatile Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
`volatile` 키워드: CPU 캐시가 아닌 메인 메모리에서 직접 읽어 가시성(visibility) 보장

**가장 권장되는 구현 — Enum:**
```java
public enum Singleton {
    INSTANCE;

    public void doSomething() { }
}
// 사용: Singleton.INSTANCE.doSomething()
```
Enum은 JVM이 직렬화와 리플렉션 공격으로부터 자동 보호합니다.

### 꼬리 질문
- `volatile` 키워드가 싱글톤에 필요한 이유는 무엇인가요?
- Enum 싱글톤이 가장 권장되는 이유는 무엇인가요?

### 실무 사례
Spring에서 `@Bean`은 기본적으로 singleton scope로 관리되며, ApplicationContext가 static 변수처럼 단일 인스턴스를 보관합니다.
