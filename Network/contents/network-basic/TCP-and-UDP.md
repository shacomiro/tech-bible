# TCP와 UDP

1. [TCP/IP 전송계층](#tcpip-전송계층)
2. [TCP(Transmission Control Protocol)](#tcptransmission-control-protocol)
3. [UDP(User Datagram Protocol)](#udpuser-datagram-protocol)
4. [3-Way Handshake](#3-way-handshake)
5. [4-Way Handshake](#4-way-handshake)
6. [참고 자료](#참고-자료)

## TCP/IP 전송계층

TCP/IP는 인터넷에서 사용하는 프로토콜 그룹을 칭한다. TCP/IP는 응용 계층, 전송 계층, 네트워크 계층, 데이터 링크 계층, 물리 계층의 5개 계층으로 나뉜다.

그 중에서 전송 계층은 **두 응용 계층 사이에서의 Process-To-Process 통신을 제공**한다. 전송 계층은 응용 계층으로부터 메시지를 받아 전송 계층 패킷으로 캡슐화하여 전송한다. 이를 세그먼트(Segment) 또는 데이터그램(Datagram)이라 부른다.

> **Process-To-Process 통신**
>
> 프로세스(Prcoess)란 응용 계층에서 구동중인 프로그램을 의미한다. 각각의 프로세스는 자신의 고유한 포트 번호(Port Number)를 갖는다.  
> 프로세스들간의 통신을 실질적으로 가능하게 해주는 것이 전송 계층이다.

> **패킷**
>
> 두 종단 사이의 통신은 '패킷'이라고 불리는 데이터 블록에 의해 이루어진다.

전송 계층에서 사용되는 주요 프로토콜은 TCP와 UDP이다. TCP(Transmission Control Protocol)는 **연결형**, **신뢰성** 전송 프로토콜이다. TCP로 전송하는 패킷을 세그먼트(Segment)라고 부른다. UDP(User Datagram Protocol)는 **비연결형**, **비신뢰성** 전송 프로토콜이다. UDP로 전송하는 패킷을 데이터그램(Datagram)이라고 한다.

## TCP(Transmission Control Protocol)

TCP는 **연결형**, **신뢰성** 전송 프로토콜이다.

- **연결지향적 서비스**를 제공하기 위해 데이터를 전송하기 전에 먼저 두 호스트의 전송 계층 사이에 **논리적 연결을 설립**한다. 그 후에 **데이터를 전송**하며 데이터 전송이 완료되면 **연결을 해제**한다.
  - TCP의 통신은 이처럼 [**Connection Setup**](#3-way-handshake), **Data Transfer**, [**Connection Termination**](#4-way-handshake)의 세 단계로 나뉜다.
- **신뢰성 있는 서비스**를 제공하기 위해 TCP가 전체 스트림을 순서에 맞고 오류 없이, 또한 부분적인 손실이나 중복 없이 전송하는 것을 보장한다. 이를 위해 **오류 제어, 흐름 제어, 혼잡 제어** 등을 사용한다.
  - **흐름 제어** : 데이터를 보내는 속도와 데이터를 받는 속도의 균형을 맞추는 것을 뜻한다.
  - **오류 제어** : 훼손된 세그먼트의 감지 및 재전송, 손실된 세그먼트의 재존송, 순서가 맞지 않게 도착한 세그먼트를 정렬하고 중복된 세그먼트를 감지 및 폐기한다.
  - TCP 헤더의 체크섬(Checksum), 확인 응답, 타임-아웃 등을 통해 수행된다.
- 신뢰성을 보장하기 위해서 헤더(Header)가 더 크고 속도가 비교적 느리다는 단점이 있다.

> **TCP 사용 예시**
>
> **매우 큰 문서 파일**을 인터넷을 통해 다운로드 받는 상황을 가정한다. 이 경우 신뢰성이 보장되는 프로토콜을 사용해야 한다. 다운로드 완료된 파일의 일부분이 손실되거나 훼손되어 있으면 안되기 때문이다. 문서 파일 전송시에 발생하는 지연은 중요한 문제가 아니다. 따라서 TCP를 사용하는 것이 바람직하다.

## UDP(User Datagram Protocol)

UDP는 **비연결형**, **비신뢰성** 전송 프로토콜이다.

- **비연결형 프로토콜**이므로 **논리적 연결을 설립하지 않고 데이터그램(Datagram)을 전송**한다.
  - 3-Way Handshake 등의 세션 수립 과정이 없다.
- **비신뢰성 프로토콜**이므로 **흐름 제어, 오류 제어, 혼잡 제어를 제공하지 않는다.**
  - 이러한 단순성은 **적은 양의 오버헤드**를 갖고 수신 여부를 확인하지 않아서 속도가 빠르다는 장점이 있다.
  - 작은 메시지를 보내거나 신뢰성을 크게 고려하지 않아도 되는 산황에서 사용한다.

> **UDP 사용 예시**
>
> **라이브 방송돠 같이 실시간 상호작용을 하는 응용 프로그램**을 사용한다고 가정한다. 음성과 영상을 한 프레임씩 전송한다. 전송 계층에서 훼손되거나 손실된 프레임을 재전송 해야한다면 전체적인 지연이 발생할 것이다. 따라서 UDP를 통해 한 프레임씩 데이터그램으로 전송한다면 훼손되거나 손상된 패킷은 무시하고 나머지 패킷을 응용 프로그램으로 전달한다. 손실된 패킷으로 인해 짧은 시간동안 화면의 일부분이 공백으로 표시되더라도 대부분의 시청자들은 인식하지 못한다. 이 때는 **신뢰성보다 실시간성이 더 중요**하기에 UDP를 사용하는 것이 바람직하다.

## 3-Way Handshake

3-Way Handshake는 TCP/IP 프로토콜로 통신하기 전 정확한 정보 전송을 위해 상대방 컴퓨터와 세션을 수립하는 (연결) 과정을 의미한다. (TCP 연결 초기화)

클라이언트가 서버에게 접속을 요청하는 `SYN` 패킷을 보내면 서버는 요청을 수락하는 `ACK`를 포함하여 `SYK` + `ACK` 패킷을 클라이언트에게 발송한다. 클라이언트가 이것을 수신한 후, 다시 `ACK`를 서버에게 발송하면 연결이 이루어지고, 이로써 데이터를 주고받을 수 있게 된다.

![velog.velcdn.com-TCP_3_WAY_HANDSHAKE](https://velog.velcdn.com/images/lkdfj6/post/074d9f80-91cd-4b3a-8fc0-eb8741fa26cb/image.png)

- **SYN**
  - TCP에서 세션을 성립할 때 가장 먼저 보내는 패킷이다.
  - 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데 사용되며 초기에 시퀀스 번호를 보내게 된다.
- **ACK**
  - 상대방으로부터 패킷을 받았다는 걸 알려주는 패킷이다.
  - 수신측이 송신측 시퀀스 번호에 TCP 계층에서 길이 또는 데이터 양을 더한 것과 같은 ACK를 응답한다.

## 4-Way Handshake

3-Way Handshake를 통해 Connection Setup을 했다면, **TCP 연결을 종료**하는 **Connection Termination** 과정은 **4-Way Handshake**를 통해 이루어진다.

TCP Connection Termination은 **양방향으로 2개의 연결이 독립적으로 닫히기 때문에 4-Way 단계를 밟는다.**

![velog.velcdn.com-TCP_4_WAY_HANDSHAKE](https://velog.velcdn.com/images/lkdfj6/post/c4e41542-2949-46da-9529-5659d8725e40/image.png)

1. 클라이언트 프로세스에서 Active Close를 하면, 클라이언트 TCP에서 `FIN` 세그먼트를 보낸다.
2. 서버는 `FIN` 세그먼트를 받았다는 응답에 대한 `ACK`를 클라이언트로 보낸다. 이때, 서버 내의 프로세스에게 EOF를 보내지만, 아직 프로세스는 CLose되지 않을 수 있다.
3. 서버 프로세스로부터 Passive Close를 받으면 서버 TCP에서 `FIN` 세그먼트를 클라이언트 TCP에게 보낸다.
4. 서버 TCP가 `ACK`를 받으면 연결이 종료된다.

## 참고 자료

- [TCP vs UDP](https://velog.io/@lkdfj6/TCP-vs-UDP)