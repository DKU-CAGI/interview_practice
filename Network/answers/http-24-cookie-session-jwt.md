## Q. HTTPS란 무엇이고, TLS 핸드셰이크 과정을 설명하시오.

### 핵심 답변

**HTTPS**는 애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 **SSL/TLS 계층을 넣은** 신뢰할 수 있는 HTTP 요청을 말합니다. 이를 통해 **통신을 암호화**합니다.

**SSL/TLS**: 전송 계층에서 보안을 제공하는 프로토콜. 클라이언트와 서버가 통신할 때 제3자가 메시지를 도청하거나 변조하지 못하도록 함

### 보충 설명

**TLS 1.3 핸드셰이크 과정:**

```
클라이언트                          서버
    |                               |
    |── ClientHello (+KeyShare) ───→|
    |                               |
    |←── ServerHello (+KeyShare) ───|
    |←── EncryptedExtensions ───────|
    |←── Certificate ───────────────|
    |←── CertificateVerify ─────────|
    |←── Finished ──────────────────|
    |←── [Application Data] ────────|
    |                               |
    |── Finished ───────────────────→|
    |── [Application Data] ─────────→|
    |←──────── Application Data ────→|
```

클라이언트와 서버가 키를 공유하고 이를 기반으로 인증, 인증 확인 등의 작업이 일어나며, **1-RTT** 후 데이터를 송수신

**사이퍼 슈트(Cipher Suite)**: 프로토콜, AEAD 사이퍼 모드, 해싱 알고리즘이 나열된 규약
- 예: TLS_AES_128_GCM_SHA256 = TLS(프로토콜) + AES_128_GCM(AEAD 사이퍼 모드) + SHA256(해싱 알고리즘)

**인증 메커니즘**: CA(Certificate Authorities)에서 발급한 인증서 기반. CA에서 발급한 인증서는 '공개키'를 클라이언트에 제공하고, 사용자가 접속한 서버가 신뢰할 수 있는 서버임을 보장

**HTTPS가 SEO에도 도움이 되는 이유**: Google은 SSL 인증서를 강조해왔고, 동일 조건이라면 HTTPS 서비스를 하는 사이트가 SEO 순위가 높을 것이라고 공식적으로 밝힘

**HTTPS 구축 방법:**
1. 직접 CA에서 구매한 인증키 기반으로 구축
2. 서버 앞단의 HTTPS를 제공하는 로드밸런서 두기
3. 서버 앞단에 HTTPS를 제공하는 CDN 두기

### 키워드
- HTTPS, SSL/TLS, TLS 핸드셰이크, 암호화, CA, 인증서, 사이퍼 슈트, SEO
