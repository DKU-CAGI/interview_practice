## Q. 절차형 프로그래밍과 객체지향 프로그래밍의 차이는?

### 핵심 답변
절차형 프로그래밍은 순서대로 실행하는 명령어의 집합으로 프로그램을 구성하고, 객체지향 프로그래밍은 데이터와 기능을 하나로 묶은 객체들의 상호작용으로 프로그램을 구성합니다.

### 상세 설명

**비교:**
| | 절차형 | 객체지향 |
|---|---|---|
| 기본 단위 | 함수(procedure) | 객체(object) |
| 데이터 | 분리된 전역/지역 변수 | 객체 내부에 캡슐화 |
| 실행 흐름 | 위→아래 순차 실행 | 객체 간 메시지 교환 |
| 코드 재사용 | 함수 호출 | 상속, 컴포지션 |
| 대표 언어 | C, Pascal, COBOL | Java, Python, C++ |

**절차형 방식:**
```c
// C 코드: 데이터와 함수가 분리됨
typedef struct {
    char name[50];
    int age;
    double balance;
} BankAccount;

void deposit(BankAccount* account, double amount) {
    account->balance += amount;
}

void printInfo(BankAccount* account) {
    printf("이름: %s, 잔액: %.2f\n", account->name, account->balance);
}

// 사용: 데이터 구조를 함수에 전달
BankAccount acc = {"홍길동", 30, 10000.0};
deposit(&acc, 5000.0);
printInfo(&acc);
```

**OOP 방식:**
```java
// 데이터와 함수가 하나의 객체로 묶임
class BankAccount {
    private String name;
    private int age;
    private double balance; // 캡슐화

    BankAccount(String name, int age, double initial) {
        this.name = name;
        this.age = age;
        this.balance = initial;
    }

    void deposit(double amount) { balance += amount; } // 메서드

    @Override
    public String toString() { return name + ": " + balance; }
}

BankAccount acc = new BankAccount("홍길동", 30, 10000.0);
acc.deposit(5000.0);
System.out.println(acc);
```

**각 패러다임이 적합한 상황:**
- **절차형**: 스크립트, 데이터 파이프라인, 임베디드 시스템 (성능 중시)
- **OOP**: 복잡한 비즈니스 로직, 대규모 엔터프라이즈 애플리케이션
- **혼용**: 현대 언어(Python, JavaScript)는 두 패러다임 모두 지원

### 꼬리 질문
- 절차형 언어인 C로도 OOP를 구현할 수 있나요?
- 함수형 프로그래밍은 절차형과 OOP 중 어디에 가깝나요?

### 실무 사례
Node.js 스크립트는 절차형이 간결하고, Spring 백엔드는 OOP가 적합합니다. Go언어는 OOP 완전 지원 없이 인터페이스와 컴포지션으로 유사한 효과를 냅니다.
