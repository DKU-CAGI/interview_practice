## Q. TCP 3-way handshake와 4-way handshake를 설명하고 TIME_WAIT가 필요한 이유를 설명하시오.

### 핵심 답변

**3-way handshake (TCP 연결 성립):**

```
클라이언트 (CLOSED)                    서버 (LISTEN)
    |                                      |
    |── ① SYN (클라이언트 ISN: 12010) ──→ |
    | (SYN-SENT)                 (SYN-RECEIVED)
    |                                      |
    |←── ② SYN+ACK (서버 ISN: 5000, 승인번호: 12011) ──|
    | (ESTABLISHED)                        |
    |                                      |
    |── ③ ACK (승인번호: 5001) ──────────→ |
    |                                (ESTABLISHED)
```

1. **SYN**: 클라이언트가 서버에 클라이언트의 ISN을 담아 SYN을 전송
2. **SYN + ACK**: 서버가 클라이언트의 SYN을 수신하고 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN + 1을 전송
3. **ACK**: 클라이언트가 서버의 ISN + 1한 값인 승인번호를 담아 ACK를 서버에 전송

**4-way handshake (TCP 연결 해제):**

```
클라이언트                             서버
    |── ① FIN ──────────────────────→ |
    | (FIN_WAIT_1)              (CLOSE_WAIT)
    |←── ② ACK ──────────────────── ─|
    | (FIN_WAIT_2)                     |
    |←── ③ FIN ───────────────────── ─|
    |                          (LAST_ACK)
    | (TIME_WAIT)                      |
    |── ④ ACK ──────────────────────→ |
    |                             (CLOSED)
    | (일정 시간 대기 후 CLOSED)
```

1. 클라이언트가 FIN으로 설정된 세그먼트를 전송, FIN_WAIT_1 상태
2. 서버는 ACK를 전송, CLOSE_WAIT 상태. 클라이언트는 FIN_WAIT_2 상태
3. 서버는 ACK를 보내고 일정 시간 이후 FIN 세그먼트를 전송
4. 클라이언트는 TIME_WAIT 상태가 되어 ACK를 보냄. 서버는 CLOSED 상태

### 보충 설명

**TIME_WAIT가 필요한 이유:**

1. **지연 패킷을 대비하기 위함**: 패킷이 늦게 도달하고 이를 처리하지 못하면 데이터 무결성 문제 발생. 예: 전체 데이터가 100일 때 일부 데이터인 50만 들어오는 현상 방지

2. **두 장치가 연결이 닫혔는지 확인하기 위함**: LAST_ACK 상태에서 닫히게 되면 새로운 연결을 하려고 할 때 장치는 줄곧 LAST_ACK로 되어 있어 접속 오류 발생

- TIME_WAIT: 소켓이 바로 소멸되지 않고 일정 시간 유지되는 상태 (CentOS6, 우분투: 60초 / 윈도우: 4분)

**ISN(Initial Sequence Numbers)**: 초기 네트워크 연결을 할 때 할당된 32비트 고유 시퀀스 번호

**데이터 무결성(data integrity)**: 데이터의 정확성과 일관성을 유지하고 보증하는 것

### 키워드
- 3-way handshake, 4-way handshake, SYN, ACK, FIN, TIME_WAIT, ISN, 데이터 무결성
