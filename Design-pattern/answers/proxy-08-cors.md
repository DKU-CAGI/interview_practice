## Q. CORS 문제를 프록시 서버로 어떻게 해결할 수 있나요?

### 핵심 답변
CORS는 브라우저의 동일 출처 정책(SOP)으로 인해 다른 도메인의 API를 직접 호출할 수 없는 문제입니다. 프록시 서버가 클라이언트 대신 API를 호출하고 결과를 반환하면, 브라우저는 같은 출처로 인식하여 CORS 오류가 발생하지 않습니다.

### 상세 설명

**CORS 발생 원리:**
```
브라우저 → frontend.com에서 api.other.com 호출 → CORS 오류
(다른 출처: 프로토콜/도메인/포트 중 하나라도 다르면)
```

**프록시 서버로 해결:**
```
브라우저 → frontend.com/api/* → [Proxy] → api.other.com
(브라우저 입장에선 같은 출처인 frontend.com과 통신)
```

**1. nginx 리버스 프록시 설정:**
```nginx
server {
    server_name frontend.com;

    location /api/ {
        proxy_pass https://api.other.com/;
        proxy_set_header Host api.other.com;
        add_header 'Access-Control-Allow-Origin' 'https://frontend.com';
    }
}
```

**2. 개발 환경: webpack-dev-server / Vite 프록시**
```javascript
// vite.config.js
export default {
    server: {
        proxy: {
            '/api': {
                target: 'https://api.other.com',
                changeOrigin: true,
                rewrite: path => path.replace(/^\/api/, '')
            }
        }
    }
};
```

**3. Node.js Express 프록시**
```javascript
const { createProxyMiddleware } = require('http-proxy-middleware');

app.use('/api', createProxyMiddleware({
    target: 'https://api.other.com',
    changeOrigin: true
}));
```

**프록시 없이 CORS 해결하는 방법:**
- API 서버에서 `Access-Control-Allow-Origin` 헤더 추가 (가장 올바른 방법)
- JSONP (GET 요청만, 레거시)
- CORS 브라우저 확장 프로그램 (개발 환경 한정)

### 꼬리 질문
- CORS Preflight 요청이란 무엇인가요?
- `Access-Control-Allow-Origin: *`의 보안 위험은 무엇인가요?

### 실무 사례
React 개발 환경에서 Create React App의 `package.json`에 `"proxy": "http://localhost:4000"` 설정이 이 방식입니다.
