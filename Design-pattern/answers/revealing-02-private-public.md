## Q. private과 public 접근 제어를 JavaScript에서 어떻게 구현하나요?

### 핵심 답변
JavaScript에서 접근 제어는 클로저(IIFE), ES6 `class`의 `#` 프라이빗 필드, `WeakMap`, 심볼(Symbol) 등으로 구현합니다.

### 상세 설명

**1. 클로저를 이용한 접근 제어 (ES5 방식)**
```javascript
function BankAccount(initialBalance) {
    let balance = initialBalance; // private

    return {
        deposit(amount) { balance += amount; }, // public
        withdraw(amount) {
            if (amount > balance) throw new Error('잔액 부족');
            balance -= amount;
        },
        getBalance() { return balance; }, // public getter
    };
}

const account = BankAccount(10000);
account.deposit(5000);
console.log(account.getBalance()); // 15000
console.log(account.balance); // undefined (private)
```

**2. ES2022 클래스 Private 필드 (#)**
```javascript
class BankAccount {
    #balance; // private 필드 (외부 접근 시 SyntaxError)
    #transactionHistory = [];

    constructor(initial) {
        this.#balance = initial;
    }

    deposit(amount) {
        this.#balance += amount;
        this.#log('입금', amount); // private 메서드 호출
    }

    #log(type, amount) { // private 메서드
        this.#transactionHistory.push({ type, amount });
    }

    get balance() { return this.#balance; } // public getter
}

const acc = new BankAccount(10000);
acc.deposit(5000);
acc.#balance; // SyntaxError: Private field
```

**3. WeakMap을 이용한 방법**
```javascript
const _private = new WeakMap();

class User {
    constructor(name, password) {
        _private.set(this, { password });
        this.name = name; // public
    }

    checkPassword(input) {
        return _private.get(this).password === input; // private 접근
    }
}
```

**4. Symbol을 이용한 방법** (완전한 private이 아님)
```javascript
const _id = Symbol('id');

class User {
    constructor(id) {
        this[_id] = id; // 외부에서 Symbol 없이 접근 불가
    }
}
```

### 꼬리 질문
- ES6 `#` 프라이빗 필드와 TypeScript의 `private` 키워드의 차이는?
- WeakMap으로 private 구현 시 이점은?

### 실무 사례
TypeScript 사용 시 `private` 키워드를 쓰지만, 런타임(JavaScript)에서는 `#` private 필드나 클로저로 실제 제한됩니다.
