## Q. 전략 패턴을 사용하면 어떤 이점이 있나요?

### 핵심 답변
전략 패턴은 알고리즘을 캡슐화하여 런타임 교체, 조건문 제거, OCP 준수, 테스트 용이성이라는 이점을 제공합니다.

### 상세 설명

**1. 조건문(if-else, switch) 제거**
```javascript
// Before: 조건문 남용
function compress(data, type) {
    if (type === 'zip') return compressZip(data);
    else if (type === 'gzip') return compressGzip(data);
    else if (type === 'bzip2') return compressBzip2(data);
}

// After: 전략 패턴
const strategies = {
    zip: new ZipStrategy(),
    gzip: new GzipStrategy(),
    bzip2: new Bzip2Strategy(),
};
const compressed = strategies[type].compress(data);
```

**2. OCP(개방-폐쇄 원칙) 준수**
새 알고리즘 추가 시 기존 코드 수정 없이 새 전략 클래스만 추가

**3. 런타임 전략 교체**
```javascript
const sorter = new Sorter(new BubbleSort());
// ... 데이터가 크다고 판단되면
sorter.setStrategy(new QuickSort()); // 런타임에 교체
```

**4. 단위 테스트 용이**
각 전략을 독립적으로 테스트 가능:
```javascript
test('QuickSort는 정렬된 배열을 반환해야 한다', () => {
    const qs = new QuickSort();
    expect(qs.sort([3, 1, 2])).toEqual([1, 2, 3]);
});
```

**5. 코드 재사용성**
동일한 전략을 여러 Context에서 재사용 가능

### 장단점
- **장점**: 유연성, OCP 준수, 테스트 용이, 조건문 감소
- **단점**: 전략 클래스 수 증가, 클라이언트가 전략을 선택해야 함

### 꼬리 질문
- 전략이 너무 많아지면 어떻게 관리하나요? (팩토리 패턴 결합)
- 함수형 프로그래밍에서 전략 패턴은 어떻게 표현되나요?

### 실무 사례
Jest의 각종 matcher, Spring Security의 `AuthenticationStrategy`, webpack의 다양한 loader/plugin 설정이 모두 전략 패턴을 활용합니다.
