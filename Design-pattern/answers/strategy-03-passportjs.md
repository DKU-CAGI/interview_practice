## Q. passport.js에서 전략 패턴이 어떻게 활용되는지 설명해주세요.

### 핵심 답변
passport.js는 인증 로직을 독립된 "전략(Strategy)" 모듈로 분리합니다. `passport.use()`로 전략을 등록하고, `passport.authenticate()`로 어떤 전략을 사용할지 지정합니다.

### 상세 설명

**passport.js의 전략 패턴 구조:**
```
Context: passport (전략 보유 및 위임)
Strategy 인터페이스: passport.Strategy
ConcreteStrategy: LocalStrategy, GoogleStrategy, JwtStrategy ...
```

**LocalStrategy 사용 예:**
```javascript
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;

// 전략 등록 (passport.use = Context에 전략 설정)
passport.use(new LocalStrategy(
    { usernameField: 'email' },
    async (email, password, done) => {
        const user = await User.findOne({ email });
        if (!user || !user.validatePassword(password)) {
            return done(null, false, { message: '인증 실패' });
        }
        return done(null, user);
    }
));

// 전략 실행 (authenticate = Context가 전략에 위임)
app.post('/login', passport.authenticate('local', {
    successRedirect: '/dashboard',
    failureRedirect: '/login',
}));
```

**JWT 전략 추가 (전략 교체의 유연성):**
```javascript
const JwtStrategy = require('passport-jwt').Strategy;

passport.use(new JwtStrategy(jwtOptions, (payload, done) => {
    User.findById(payload.id, (err, user) => {
        done(err, user || false);
    });
}));

// API 엔드포인트에서 JWT 전략 사용
app.get('/api/profile', passport.authenticate('jwt'), (req, res) => {
    res.json(req.user);
});
```

**핵심 이점:**
- 인증 방식(로컬, OAuth, JWT 등)을 독립적으로 추가/교체
- 여러 전략을 동시에 등록하고 라우트별로 선택
- 인증 로직이 비즈니스 로직과 분리됨

### 꼬리 질문
- passport.js에서 `done` 콜백의 역할은 무엇인가요?
- 여러 인증 전략을 동시에 사용해야 할 때 어떻게 구성하나요?

### 실무 사례
일반 로그인(LocalStrategy), 소셜 로그인(Google/Kakao OAuth Strategy), API 인증(JWT Strategy)을 같은 passport 인스턴스에 모두 등록하고 라우트별로 선택적으로 사용합니다.
