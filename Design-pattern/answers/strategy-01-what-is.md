## Q. 전략 패턴이란 무엇이며, 정책 패턴과 어떤 관계가 있나요?

### 핵심 답변
전략 패턴은 동일한 문제를 해결하는 알고리즘군을 캡슐화하고, 런타임에 교체 가능하도록 하는 행동(Behavioral) 패턴입니다. 정책 패턴(Policy Pattern)이라고도 불립니다.

### 상세 설명
**핵심 구조:**
- **Context**: 전략 객체를 보유하고, 전략에 작업을 위임
- **Strategy(Interface)**: 알고리즘의 공통 인터페이스 정의
- **ConcreteStrategy**: 실제 알고리즘 구현체

```javascript
// Strategy 인터페이스
class SortStrategy {
    sort(data) { throw new Error('구현 필요'); }
}

// ConcreteStrategy
class BubbleSort extends SortStrategy {
    sort(data) { /* 버블 정렬 */ return data; }
}

class QuickSort extends SortStrategy {
    sort(data) { /* 퀵 정렬 */ return data; }
}

// Context
class Sorter {
    constructor(strategy) {
        this.strategy = strategy;
    }

    setStrategy(strategy) {
        this.strategy = strategy; // 런타임에 교체
    }

    sort(data) {
        return this.strategy.sort(data);
    }
}

const sorter = new Sorter(new BubbleSort());
sorter.sort([3, 1, 2]);

sorter.setStrategy(new QuickSort()); // 전략 교체
sorter.sort([3, 1, 2]);
```

**정책 패턴과의 관계:**
전략 패턴과 정책 패턴은 동일한 패턴의 다른 이름입니다. "전략"은 알고리즘 관점, "정책"은 비즈니스 규칙 관점에서 부르는 명칭입니다.

### 장단점
- **장점**: 알고리즘 교체 용이, OCP 준수, 조건문 제거
- **단점**: 전략 클래스 수 증가, 클라이언트가 전략 차이를 알아야 함

### 꼬리 질문
- 전략 패턴과 팩토리 패턴을 함께 사용하는 경우를 설명해주세요.
- 전략 패턴과 상태 패턴의 차이는 무엇인가요?

### 실무 사례
Java의 `Comparator`, JavaScript의 `Array.sort((a, b) => a - b)` (비교 함수가 전략), Spring Security의 인증 전략이 전략 패턴의 예입니다.
