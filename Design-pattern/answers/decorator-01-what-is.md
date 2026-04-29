## Q. 데코레이터 패턴은 어떤 문제를 해결하나요?

### 핵심 답변
데코레이터 패턴은 상속 없이 객체에 동적으로 기능을 추가하는 구조 패턴입니다. 상속으로 인한 클래스 폭발(Class Explosion) 문제를 해결하고, 기능을 조합하는 유연성을 제공합니다.

### 상세 설명

**상속의 문제 (클래스 폭발):**
커피에 우유/설탕/시럽을 조합하면 2^n 개의 클래스가 필요:
- Coffee, CoffeeWithMilk, CoffeeWithSugar, CoffeeWithMilkAndSugar...

**데코레이터 패턴으로 해결:**
```javascript
// 기본 컴포넌트
class Coffee {
    getCost() { return 3000; }
    getDescription() { return '아메리카노'; }
}

// 데코레이터 베이스
class CoffeeDecorator {
    constructor(coffee) {
        this.coffee = coffee;
    }
    getCost() { return this.coffee.getCost(); }
    getDescription() { return this.coffee.getDescription(); }
}

// 구체 데코레이터
class MilkDecorator extends CoffeeDecorator {
    getCost() { return this.coffee.getCost() + 500; }
    getDescription() { return this.coffee.getDescription() + ', 우유'; }
}

class SugarDecorator extends CoffeeDecorator {
    getCost() { return this.coffee.getCost() + 300; }
    getDescription() { return this.coffee.getDescription() + ', 설탕'; }
}

class VanillaSyrupDecorator extends CoffeeDecorator {
    getCost() { return this.coffee.getCost() + 700; }
    getDescription() { return this.coffee.getDescription() + ', 바닐라 시럽'; }
}

// 조합 사용
let coffee = new Coffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);
coffee = new VanillaSyrupDecorator(coffee);

console.log(coffee.getDescription()); // 아메리카노, 우유, 설탕, 바닐라 시럽
console.log(coffee.getCost()); // 4500
```

**프록시 패턴과의 차이:**
- 프록시: 접근 제어 목적, 인터페이스 동일, 주로 1개의 래퍼
- 데코레이터: 기능 추가 목적, 인터페이스 동일, 여러 겹으로 쌓기 가능

### 장단점
- **장점**: 클래스 폭발 방지, OCP 준수, 런타임 기능 조합
- **단점**: 많은 데코레이터 래핑 시 디버깅 어려움, 순서 의존성

### 꼬리 질문
- TypeScript 데코레이터(`@Decorator`)와 데코레이터 패턴의 관계는?
- 데코레이터 패턴이 AOP(Aspect-Oriented Programming)와 어떻게 연관되나요?

### 실무 사례
Node.js의 스트림 파이프라인(`fs.createReadStream().pipe(zlib.createGzip()).pipe(res)`)이 데코레이터 패턴의 예입니다. TypeScript의 `@Injectable`, `@Component` 같은 메타데이터 데코레이터도 관련 개념입니다.
