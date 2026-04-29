## Q. 전략 패턴과 상태 패턴의 차이는 무엇인가요?

### 핵심 답변
전략 패턴은 클라이언트가 알고리즘을 선택하여 주입하고, 상태 패턴은 객체가 내부 상태에 따라 스스로 행동을 변경합니다. 구조는 유사하지만 의도와 사용 맥락이 다릅니다.

### 상세 설명

**비교:**
| | 전략 패턴 | 상태 패턴 |
|---|---|---|
| 누가 교체? | 클라이언트(외부) | 객체 자신(내부) |
| 교체 시점 | 언제든지 | 상태 전이 시 |
| 상태 의존성 | 전략끼리 독립 | 상태끼리 전이 관계 존재 |
| 의도 | 알고리즘 교체 | 상태에 따른 행동 변화 |

**상태 패턴 예시 (자동 상태 전이):**
```javascript
class TrafficLight {
    constructor() {
        this.state = new RedState(this);
    }

    setState(state) { this.state = state; }
    next() { this.state.next(); } // 상태가 스스로 전이 결정
}

class RedState {
    constructor(light) { this.light = light; }
    next() {
        console.log('초록불로 전환');
        this.light.setState(new GreenState(this.light)); // 스스로 전이
    }
}

class GreenState {
    constructor(light) { this.light = light; }
    next() {
        console.log('노란불로 전환');
        this.light.setState(new YellowState(this.light));
    }
}
```

**전략 패턴 비교 (클라이언트가 교체):**
```javascript
const sorter = new Sorter(new QuickSort()); // 클라이언트가 선택
sorter.setStrategy(new MergeSort()); // 클라이언트가 교체
```

**핵심 질문: "누가 알고리즘을 선택하는가?"**
- 클라이언트(외부) → 전략 패턴
- 객체 자신(내부, 상태에 따라) → 상태 패턴

### 꼬리 질문
- 상태 패턴을 전략 패턴으로 리팩토링할 수 있는 경우는 언제인가요?
- 상태 패턴에서 상태 전이를 Context가 관리하는 방식과 State가 관리하는 방식의 차이는?

### 실무 사례
주문 상태 관리(결제대기→결제완료→배송중→배송완료)는 상태 패턴, 결제 수단 선택(카드/페이)은 전략 패턴으로 구현합니다.
