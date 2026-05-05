## Q. HTTP/3와 QUIC의 특징, HTTP/2와의 차이를 설명하시오.

### 핵심 답변

**HTTP/3**는 HTTP/1.1 및 HTTP/2와 함께 World Wide Web에서 정보를 교환하는 데 사용되는 HTTP의 세 번째 버전입니다.

**핵심 차이**: TCP 위에서 동작하는 HTTP/2와 달리, HTTP/3는 **QUIC이라는 계층 위에서 동작하며 UDP 기반**으로 동작합니다.

| 구분 | HTTP/2 | HTTP/3 |
|------|--------|--------|
| 기반 프로토콜 | TCP | UDP (QUIC) |
| 초기 연결 | TCP 3-way handshake 필요 | 불필요 (1-RTT) |
| HOL Blocking | TCP 수준에서 여전히 발생 | QUIC 레벨에서 해결 |

### 보충 설명

**QUIC의 주요 장점:**

1. **초기 연결 설정 시 지연 시간 감소**
   - TCP를 사용하지 않아 번거로운 3-way handshake 과정 불필요
   - 첫 연결 설정에 1-RTT만 소요됨
   - 클라이언트가 서버에 신호를 한 번 주고, 서버가 거기에 응답하기만 하면 바로 통신 시작

2. **멀티플렉싱 지원** (HTTP/2에서 이어받음)

3. **순방향 오류 수정 메커니즘 (FEC, Forward Error Correction)**
   - 전송한 패킷이 손실되었다면 수신 측에서 에러를 검출하고 수정하는 방식
   - 열악한 네트워크 환경에서도 낮은 패킷 손실률

**RTT 비교:**
```
TCP + TLS HTTPS: 200ms + 300ms = 최소 500ms
QUIC HTTPS: 0ms + 100ms = 100ms (재방문 시 0-RTT)
```

TLS 1.3 기반 0-RTT: 사용자가 이전에 방문한 사이트로 다시 방문하면 보안 세션을 만들 때 걸리는 통신을 하지 않아도 됨

### 키워드
- HTTP/3, QUIC, UDP, 1-RTT, 0-RTT, FEC, 멀티플렉싱
