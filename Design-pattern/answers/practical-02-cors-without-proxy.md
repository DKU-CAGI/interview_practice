## Q. 프록시 서버를 사용하지 않고 CORS 문제를 해결할 수 있나요?

### 핵심 답변
네, 가장 올바른 방법은 API 서버에서 `Access-Control-Allow-Origin` 헤더를 설정하는 것입니다. 이 외에도 JSONP, 서버 사이드 요청 등의 방법이 있지만, 헤더 설정이 표준적인 방법입니다.

### 상세 설명

**방법 1: API 서버에서 CORS 헤더 설정 (가장 권장)**
```javascript
// Express.js
const cors = require('cors');

// 특정 도메인만 허용 (권장)
app.use(cors({
    origin: ['https://frontend.com', 'http://localhost:3000'],
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    credentials: true, // 쿠키 포함 요청 허용
}));

// 모든 도메인 허용 (주의 필요)
app.use(cors()); // Access-Control-Allow-Origin: *
```

```java
// Spring
@CrossOrigin(origins = "https://frontend.com")
@RestController
public class UserController { /* ... */ }

// 전역 설정
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
            .allowedOrigins("https://frontend.com")
            .allowedMethods("*");
    }
}
```

**방법 2: 프록시 서버 사용 (개발 환경)**
```javascript
// Vite
export default {
    server: { proxy: { '/api': 'http://api.example.com' } }
};
```

**방법 3: JSONP (레거시, GET 요청만 가능)**
```javascript
// 스크립트 태그는 SOP 제외됨 (레거시 방법)
// 보안 취약점 있어 사용 비권장
```

**방법 4: 서버 사이드에서 요청**
```javascript
// Node.js 서버가 다른 API를 호출 (서버→서버는 SOP 적용 안 됨)
app.get('/proxy', async (req, res) => {
    const data = await fetch('https://api.other.com/data');
    res.json(await data.json());
});
```

**CORS 설정 시 보안 주의사항:**
- `Access-Control-Allow-Origin: *`와 `credentials: true` 동시 사용 불가
- 허용할 출처를 최소화 (와일드카드 * 사용 최소화)
- Preflight 요청(OPTIONS) 처리 필요

### 꼬리 질문
- `Access-Control-Allow-Origin: *`의 보안 위험은 무엇인가요?
- CORS Preflight 요청을 캐싱하는 방법은?

### 실무 사례
백엔드와 프론트엔드가 다른 팀이라면, API 서버에서 허용 출처를 환경 변수로 관리하여 개발/스테이징/프로덕션에서 다른 설정을 사용합니다.
