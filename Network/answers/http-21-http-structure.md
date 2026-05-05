## Q. HTTP/1.0의 문제점과 HTTP/1.1이 이를 어떻게 개선했는지 설명하시오.

### 핵심 답변

**HTTP/1.0의 문제점 - RTT 증가:**
- 기본적으로 **한 연결당 하나의 요청을 처리**하도록 설계
- 서버로부터 파일을 가져올 때마다 TCP의 3-way handshake를 계속 열어야 함
- 매번 연결할 때마다 **RTT(Round Trip Time)가 증가**하여 서버에 부담이 많아지고 사용자 응답 시간이 길어짐

**HTTP/1.1의 개선 - keep-alive:**
- 매번 TCP 연결을 하는 것이 아니라 **한 번 TCP 초기화를 한 이후 `keep-alive` 옵션으로 여러 개의 파일을 송수신**할 수 있게 개선
- 참고: HTTP/1.0에도 keep-alive가 있었지만, HTTP/1.1부터 표준화되어 기본 옵션으로 설정됨

### 보충 설명

**HTTP/1.1의 남은 문제점:**

1. **HOL Blocking (Head Of Line Blocking)**: 같은 큐에 있는 패킷이 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상
   - image.jpg가 느리게 받아진다면 그 뒤에 있는 styles.css, data.xml도 대기하게 됨

2. **무거운 헤더 구조**: 쿠키 등 많은 메타데이터가 들어 있고 압축이 되지 않아 무거움

**HTTP/1.0 vs HTTP/1.1:**
```
HTTP/1.0:  [TCP 핸드셰이크] GET → Response [TCP 종료] [TCP 핸드셰이크] GET → Response [TCP 종료]
HTTP/1.1:  [TCP 핸드셰이크] GET → Response → GET → Response → GET → Response [TCP 종료]
```

**RTT(Round Trip Time)**: 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간 (패킷 왕복 시간)

### 키워드
- HTTP/1.0, HTTP/1.1, RTT, keep-alive, HOL Blocking, 헤더
