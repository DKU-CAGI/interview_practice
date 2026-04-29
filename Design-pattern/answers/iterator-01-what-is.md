## Q. 이터레이터 패턴이란 무엇이며, 어떤 문제를 해결하나요?

### 핵심 답변
이터레이터 패턴은 컬렉션(배열, 트리, 그래프 등)의 내부 표현을 노출하지 않고 요소를 순차적으로 접근할 수 있는 통일된 인터페이스를 제공하는 행동 패턴입니다.

### 상세 설명

**해결하는 문제:**
- 배열, 연결 리스트, 트리 등 자료구조마다 다른 순회 방법
- 클라이언트가 자료구조의 내부 구현을 알아야 하는 문제
- 다양한 순회 방식(정방향, 역방향, 깊이 우선 등) 지원

**이터레이터 프로토콜:**
```javascript
// 이터레이터는 next() 메서드를 가진 객체
const iterator = {
    data: [1, 2, 3],
    index: 0,

    next() {
        if (this.index < this.data.length) {
            return { value: this.data[this.index++], done: false };
        }
        return { value: undefined, done: true };
    }
};

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

**이터러블 객체 만들기:**
```javascript
class Range {
    constructor(start, end) {
        this.start = start;
        this.end = end;
    }

    // Symbol.iterator로 이터러블 프로토콜 구현
    [Symbol.iterator]() {
        let current = this.start;
        const end = this.end;
        return {
            next() {
                if (current <= end) {
                    return { value: current++, done: false };
                }
                return { value: undefined, done: true };
            }
        };
    }
}

for (const num of new Range(1, 5)) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

### 장단점
- **장점**: 자료구조와 순회 로직 분리, 통일된 인터페이스, OCP 준수
- **단점**: 간단한 컬렉션에는 과도한 복잡성

### 꼬리 질문
- 이터레이터와 제너레이터의 차이는 무엇인가요?
- 이터레이터 패턴이 복합(Composite) 패턴과 함께 사용되는 경우는?

### 실무 사례
JavaScript의 `for...of`, 배열/Map/Set/String의 이터레이터, Python의 `for...in`, Java의 `Iterator<T>` 인터페이스가 모두 이터레이터 패턴 구현입니다.
