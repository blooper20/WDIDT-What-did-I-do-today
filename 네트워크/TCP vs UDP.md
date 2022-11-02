# TCP & UDP

- 모두 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달해주는 IP 프로토콜을 기반으로 구현되어져있다.
- 모두 전송 계층에 사용되는 프로토콜이다.

## TCP

- 장치들 사이에 논리적인 접속을 성립(Establish)하기 위하여 연결을 설정하여 신뢰성을 보장하는 연결형 서비스이다.
- 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 옥텟(데이터, 메세지, 세그먼트라는 블록단위)를 안정적으로, 순서대로, 에러없이 교환할 수 있게한다.
  ![](https://velog.velcdn.com/images/blooper20/post/24396d42-b093-4716-8adf-3deebf714616/image.png)

### TCP의 특징

- 연결형 서비스

```
연결형서비스로 가상 회선 방식을 제공한다.
- 3-way handshaking 과정을 통해 연결을 설정
- 4-way handshaking 과정을 통해 연결을 해제
```

- 흐름제어(Flow control)

```
데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지
- 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 막는다.
- 수신자가 윈도우 크기(Window Size)값을 통해 수신향을 정할 수 있다.
```

- 혼잡 제어(Congestion control)

```
네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지
- 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막는다.
```

- 신뢰성이 높은 전송(Reliable transmission)

```
-Dupack-based retransmission
	- 정상적인 상황에서는 ACK 값이 연속적으로 전송되어야 한다.
    - 그러나 ACK 값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청한다.
- Timeout-based restansmission
	- 일정시간동안 ACK 값이 수신을 못할 경우 재전송을 요청한다.
```

- 전이중, 점대점 방식

```
전이중(Full-Duplex)
- 전송이 양방향으로 동시에 일어날 수 있다.
점대점(Point to Point)
- 각 연결이 정확이 2개의 종단점을 가지고 있다.
```

-> 멀티캐스팅이나 브로드 캐스팅을 지원하지 않는다.

### TCP Header 정보

![](https://velog.velcdn.com/images/blooper20/post/97d8b354-0ad9-47d1-88b2-827c49fc6b3f/image.png)
응용 계층으로부터 데이터를 받은 TCP는 헤더를 추가한 후에 이를 IP로 보낸다. 헤더에는 아래 표와 같은 정보가 포함된다.
![](https://velog.velcdn.com/images/blooper20/post/0b19945f-2e04-4578-b3e7-5613938e485a/image.png)

- 제어 비트(Flag Bit) 정보
  ![](https://velog.velcdn.com/images/blooper20/post/cd9aaf2f-af41-42d7-8ece-8cc851af1eed/image.png)
- ACK 제어비트

```
- ACK는 송신측에 대하여 수신측에서 긍정 응답으로 보내지는 전송 제어용 캐릭터
- ACK 번호를 사용하여 패킷이 도착했는지 확인한다.
	-> 송신한 패킷이 제대로 도착하지 않았으면 재송신을 요구한다.
```

## UDP

- 데이터를 데이터그램 단위로 처리하는 프로토콜
- 비연결형 프로토콜 -각각의 패킷은 다른 경로로 전송되고 각각 독립적인 관계를 지니게 되어 데이터를 서로 다른 경로로 독립적으로 처리하게 된다.![](https://velog.velcdn.com/images/blooper20/post/26f866a8-a693-4605-a7b7-aa3e2a0462ef/image.png)![](https://velog.velcdn.com/images/blooper20/post/131521f5-b444-4543-8f8f-9820906af344/image.png)

### UDP 특징

- 비연결형 서비스로 데이터그램 방식을 제공한다.
- 정보를 주고 받을 때 정보를 보내거나 받는다는 신호 절차를 거치지 않는다.
- UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
- 신뢰성이 낮다
- TCP보다 속도가 빠르다
  -> 신뢰성보다는 연속성이 중요한 서비스(실시간 서비스)에 자주 사용된다.

## TCP와 UDP의 비교

![](https://velog.velcdn.com/images/blooper20/post/4f6ecd4f-9868-4925-8525-4805f0deb1ea/image.png)

---

### 참고한 블로그 링크

- [mangkyu님의 블로그](https://mangkyu.tistory.com/15)
- [hidaehyunlee님의 블로그](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)
