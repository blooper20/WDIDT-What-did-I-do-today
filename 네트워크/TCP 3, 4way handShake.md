# TCP 3,4 way handShake에 대하여

> ## TCP란?

- 연결 지향형 프로토콜 : 연속성이 있는 데이터 패킷을 주고 받을 때 사용한다.

### 특징

- 전송되는 데이터의 신뢰성 보장(흐름 제어, 혼잡 제어, 오류 제어)
- 파일전송에 주로 사용
- 가상 회선을 만들어 신뢰성을 보장

> ## TCP 3 way handShake

- 연결하고자 하는 두 장치 간의 논리적 접속을 성립하기 위해 사용하는 연결확인 방식으로, 3번의 확인 과정을 거친다고 해서 3 way handshake 라고 부른다
- TCP/IP프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다

---

### 과정

```
1. Client > Server : TCP SYN
2. Server > Client : TCP SYN ACK
3. Client > Server : TCP ACK
```

### 이 과정을 쉽게 표현한 방식

```
1. A -> B : 제 말이 잘 들릴까요?
2. B -> A : 잘들립니다. 제 말은 잘 들리십니까?
3. A -> B : 잘들리네요 ^^
```

- SYN(Synchronize Sequence Numbers): 연결 확인을 위해 보내는 무작위의 숫자값(1. 제 말이 잘 들릴까요?)
- ACK(ACKnowledgements): Client 혹은 Server로부터 받은 SYN에 1을 더해 SYN을 잘 받았다는 ACK(2. 잘들립니다. 제 말은 잘 들리십니까?)
- ISN(Initial Sequence Numbers): Client와 Server가 각각 처음으로 생성한 SYN

```
STEP 1. A클라이언트는 B서버에 접속을 요청하는 SYN 패킷을 보낸다.
이때 A클라이언트는 SYN 을 보내고 SYN/ACK 응답을 기다리는
SYN_SENT 상태가 되는 것이다.
-----------------------------------------------------------------------------------------------------------------
STEP 2. B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK 와 SYN flag 가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다린다.
이때 B서버는 SYN_RECEIVED 상태가 된다.
-----------------------------------------------------------------------------------------------------------------
STEP 3. A클라이언트는 B서버에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 되는것이다.
이때의 B서버 상태가 ESTABLISHED 이다.
```

---

> ## TCP 4 way handShake

- 3-Way handshake는 TCP의 연결을 초기화 할 때 사용한다면, 4-Way handshake는 반대로 세션을 종료하기 위해 수행되는 절차이다.

---

### TCP의 4-way Handshaking 과정

```
STEP 1. 클라이언트가 연결을 종료하겠다는 FIN플래그를 전송한다.
STEP 2. 서버는 일단 확인메시지를 보내고 자신의 통신이 끝날때까지 기다리는데 이 상태가 TIME_WAIT상태다.
STEP 3. 서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN플래그를 전송한다.
STEP 4. 클라이언트는 확인했다는 메시지를 보낸다.
```

### 이 과정을 쉽게 표현한 방식

```
1. A -> B: 나는 더 이상 보낼게 없어 이제 끊자
2. B -> A: 알겠어 나도 준비 좀 할게 기다려줘
3. B -> A: 준비 다 됐다 끊을게
4. A -> B: 알겠어~
```

```
상태        | 설명
CLOSE	   | 연결 수립을 시작하기 전의 기본 상태 (연결 없음)
ESTABLISHED| 연결의 수립이 완료된 상태, 서로 데이터를 교환할 수 있다.
CLOSE-WAIT | 상대방의 FIN(종료 요청)을 받은 상태. 상대방 FIN에 대한 ACK를 보내고 애플리케이션에 종료를 알린다.
LAST-ACK   | CLOSE-WAIT 상태를 처리 후 자신의 FIN요청을 보낸 후 FIN에 대한 ACK를 기다리는 상태.
FIN-WAIT-1 | 자신이 보낸 FIN에 대한 ACK를 기다리거나 상대방의 FIN을 기다린다.
FIN-WAIT-2 | 자신이 보낸 FIN에 대한 ACK를 받았고 상대방의 FIN을 기다린다.
CLOSING    | 상대방의 FIN에 ACK를 보냈지만 자신의 FIN에 대한 ACK를 못받은 상태
TIME-WAIT  | 모든 FIN에 대한 ACK를 받고 연결 종료가 완료된 상태. 새 연결과 겹치지 않도록 일정 시간 동안 기다린 후 CLOSED로 전이한다.
```

### Q. 그런데 만약 "Server에서 FIN을 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN패킷보다 늦게 도착하는 상황"이 발생한다면 어떻게 될까요?

**A. Client에서 세션을 종료시킨 후 뒤늦게 도착하는 패킷이 있다면 이 패킷은 Drop되고 데이터는 유실될 것입니다.
이러한 현상에 대비하여 Client는 Server로부터 FIN을 수신하더라도 일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거치게 되는데 이 과정을 "TIME_WAIT" 라고 합니다.**

---

### Time-Wait?

- 먼저 연결을 끊는 (active closer) 쪽에 생성되는 소켓으로, 혹시 모를 패킷 전송 실패에 대비하기 위하여 존재하는 소켓

---

### Time-wait이 없다면?

패킷의 손실이 발생하거나 통신자 간 연결 해제가 제대로 이루어지지 않을 수 있다!
