## Q. nginx를 프록시 서버로 사용하는 이유는 무엇인가요?

### 핵심 답변
nginx는 리버스 프록시로 사용하여 로드 밸런싱, SSL 종료, 정적 파일 서빙, 캐싱, CORS 처리 등을 애플리케이션 서버 대신 처리합니다. 가볍고 빠른 이벤트 기반 아키텍처로 고성능 처리가 가능합니다.

### 상세 설명

**nginx가 리버스 프록시로 처리하는 것들:**

**1. 로드 밸런싱**
```nginx
upstream backend {
    server app1:3000;
    server app2:3000;
    server app3:3000;
}

server {
    location / {
        proxy_pass http://backend;
    }
}
```

**2. SSL 종료 (SSL Termination)**
```nginx
server {
    listen 443 ssl;
    ssl_certificate /etc/ssl/cert.pem;

    location / {
        proxy_pass http://app:3000; # 내부는 HTTP
    }
}
```
애플리케이션 서버는 HTTP로 통신하고 SSL 처리는 nginx가 담당 → 앱 서버 부하 감소

**3. 정적 파일 직접 서빙**
```nginx
location /static/ {
    root /var/www;  # nginx가 직접 반환, 앱 서버 불필요
}
```

**4. CORS 헤더 추가**
```nginx
location /api/ {
    add_header 'Access-Control-Allow-Origin' '*';
    proxy_pass http://app:3000;
}
```

**5. 요청 제한 (Rate Limiting)**
```nginx
limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
location /api/ {
    limit_req zone=api burst=20;
}
```

**사용 이유 요약:**
- 하나의 서버에 여러 앱 서비스 가능 (포트 80/443 공유)
- 보안: 내부 서버 직접 노출 방지
- 성능: C10K 문제 해결 (이벤트 기반, 비동기 처리)

### 꼬리 질문
- nginx와 Apache의 차이점은 무엇인가요?
- nginx에서 업스트림 서버 health check를 어떻게 설정하나요?

### 실무 사례
Node.js 앱의 경우 nginx가 443(HTTPS)을 받아 3000(HTTP)으로 프록시하고, `/static`은 직접 서빙하는 구성이 일반적입니다.
