## Q. 싱글톤 패턴이란 무엇이며, 어떤 상황에서 사용하나요?

### 핵심 답변
싱글톤 패턴은 클래스의 인스턴스가 오직 하나만 생성되도록 보장하고, 이 인스턴스에 전역적으로 접근할 수 있게 하는 생성(Creational) 디자인 패턴입니다.

### 상세 설명
두 가지 핵심 조건:
1. 인스턴스가 오직 하나만 존재해야 함
2. 그 인스턴스에 전역적으로 접근 가능해야 함

**주로 사용하는 상황:**
- 데이터베이스 연결 풀 (Connection Pool)
- 로거(Logger) — 애플리케이션 전체에서 동일한 로그 스트림 사용
- 설정(Configuration) 관리 객체
- 캐시(Cache) 관리
- 이벤트 버스(Event Bus)

### 코드 예시
```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    this.value = Math.random();
    Singleton.instance = this;
  }
}

const a = new Singleton();
const b = new Singleton();
console.log(a === b); // true
console.log(a.value === b.value); // true
```

### 장단점
- **장점**: 인스턴스 중복 생성 방지, 메모리 절약, 전역 상태 일관성 유지
- **단점**: 전역 상태로 인한 예측 불가능한 버그, 테스트 어려움, 강한 결합

### 꼬리 질문
- 싱글톤 패턴이 안티패턴으로 불리는 이유는 무엇인가요?
- 멀티스레드 환경에서 싱글톤을 안전하게 구현하려면 어떻게 해야 하나요?

### 실무 사례
Spring Framework의 `@Component`, `@Service`, `@Repository`로 등록된 빈(Bean)은 기본적으로 싱글톤 스코프로 관리됩니다. Node.js에서는 `require()`의 모듈 캐싱으로 자연스럽게 싱글톤이 됩니다.
