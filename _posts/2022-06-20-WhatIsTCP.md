---
published: true
title: "TCP/IP - TCP는 무엇일까?"
categories:
  - FrontEnd
tags:
  - FrontEnd
---

오늘은 브라우저 렌더링 1화에서 언급되었던 TCP/IP 중 TCP에 대해 알아보겠습니다.

![image](https://user-images.githubusercontent.com/60723373/174483029-65b85f4b-7fab-46d4-8a6c-60319b3e1039.png)

(자~ 드가자~)

## TCP/IP란? TCP/IP라는 프로토콜이 있었나...?

제목이 붙어 있어서 좀 오해가 있을 수도 있었겠지만, 사실 "TCP/IP"라는 프로토콜은 오해의 소지가 살짝 있습니다. TCP와 IP라는 프로토콜이라고 구분 지어 생각 하는 게 맞습니다.

먼저, 위키백과에 TCP/IP를 검색해보니 대략 이렇게 나옵니다.

**인터넷 프로토콜 스위트**는 인터넷에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 **통신규약 프로토콜의 모음**이다.

인터넷 프로토콜 스위트 중 **TCP와 IP가 가장 많이 쓰이기 때문에 TCP/IP 프로토콜 스위트라고도 불린다.**

그렇습니다. 둘이 잘 쓰여서 붙여서 썼지만 둘은 프로토콜 조합입니다. 오늘은 TCP에 대해서 알아볼 것이구요.

또한, 둘은 서로 다른 계층에 있습니다.

인터넷 프로토콜 스위트의 경우, 저희가 아는 OSI 7계층에서 변형된 자신만의 계층을 가지고 있기 때문인데요. 아래의 사진을 한 번 볼까요?

![image](https://user-images.githubusercontent.com/60723373/174483755-cd141b88-9ecb-4252-ab66-fb0a83c8af6f.png)

(TCP/IP Protocol은 위에서 봤듯이 인터넷 프로토콜입니다!)

오른쪽의 TCP/IP Protocol의 4가지 계층을 보면 OSI 7 계층을 좀 더 넓게 보고 구성한 느낌입니다. 또한, 위에서 말했듯이 TCP의 경우 전송 계층에 있고, IP의 경우 Internet 계층에 있는 것을 알 수 있습니다.

## TCP는 뭘까?

TCP (전송 제어 프로토콜)은 **두 개의 호스트를 연결하고 데이터 스트림을 교환**하게 해주는 중요한 네트워크 프로토콜입니다.

아까 위에서 봤듯이 **전송 계층에 속해있고 같은 계층에는 UDP가 있습니다.**

TCP와 UDP의 차이점은 뭘까요? 간단하게 **신뢰성의 차이**입니다.

TCP의 경우 보내는 쪽에 제대로 도착했는지 응답을 받습니다. 잘못됬다고 응답이 오면 다시 보내주겠죠?

그러나, UDP의 경우 단방향 통신이라 그런 것이 없습니다. 받는 쪽이 제대로 받았는지, 잘못된 것을 받았는지는 모릅니다. 나는 보냈으니 끝이야! 이런 스탠스라고 볼 수 있겠네요.

그나마 UDP를 변호해보자면 **TCP보다 UDP가 속도는 빠르다는 것....?**. 그냥 TCP를 쓰면 될 것 같아요.

자, 다시 TCP로 돌아와서 TCP에는 4가지 정도의 특징이 있습니다.

**1. 연결형 서비스**
연결형 서비스로 가상 회선 방식을 제공합니다. **3-way handshaking으로 연결을 설정하며, 4-way handshaking으로 연결을 해제**합니다. 3-way handshaking에 대해서는 아래에서 자세히 보겠습니다.

**2. 흐름 제어**
송신 측과 수신 측의 TCP 버퍼 크기 차이로 인해 생기는 데이터 처리 속도 차이를 해결하기 위한 기법을 사용합니다.

=> 여러 방법이 존재하나, 3-way handshaking 내에서 **윈도우 크기(window size)를 설정**함으로써 해결하는 방식을 보통 씁니다.

**3. 혼잡 제어**
송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법을 사용합니다.

**4. 신뢰성이 높은 전송**
위에서도 얘기했듯이, TCP는 응답이 제대로 오지 않으면 데이터를 재전송 합니다.

재전송의 기준은 아래와 같습니다.

1. **시간 기반 재전송(Time-based Retransmission)**
   - 일정 기간동안 ACK 값이 수신을 못할 경우 재전송.
2. **중복 ACK 기반 재전송(Dup ACK - based Retransmission)**
   - 정상적인 상황에서는 ACK 값이 연속적으로 전송 되야 합니다.
   - 그러나, ACK값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청합니다.

**5. UDP보다 속도 느림**
위에서 TCP와 UDP를 비교하면서 나왔던 이야기입니다. UDP는 보내기만 하니까 당연히 TCP보다 속도가 더 빠릅니다.

**6. 전이중(Full-Duplex), 점대점(Point to Point) 방식**

- 전이중 : 전송이 양방향으로 동시에 일어날 수 있음.
- 점대점 : 각 연결이 정확히 2개의 종단점을 가지고 있음.
  => **멀티캐스팅이나 브로드캐스팅을 지원하지 않음.**

## 3, 4 - way Handshaking

자 그럼, TCP의 꽃인 3-way handshaking, 4-way handshaking도 한번 공부해봅시다.

### 3 way Handshaking

TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미합니다.

![image](https://user-images.githubusercontent.com/60723373/174486712-408fb22e-ff8f-4017-8b58-070ed6e759c7.png)

1. A클라이언트는 B서버에 접속을 요청하는 SYN 패킷을 보냅니다.

   - 이때 A클라이언트는 SYN 을 보내고 SYN/ACK 응답을 기다리는SYN_SENT 상태가 됩니다.

2. B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK 와 SYN flag 가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다립니다.

   - 이때 B서버는 SYN_RECEIVED 상태가 됩니다.

3. A클라이언트는 B서버에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 됩니다.
   - 이때의 B서버 상태가 ESTABLISHED 입니다.

### 4 - way Handshaking

4 - way Handshaking은 세션을 종료하기 위해 수행되는 절차입니다!

![image](https://user-images.githubusercontent.com/60723373/174486962-63097255-802c-4197-bda8-b08f53e44e74.png)

1. 클라이언트가 연결을 종료하겠다는 FIN플래그를 전송한다.

2. 서버는 일단 확인메시지를 보내고 자신의 통신이 끝날때까지 기다리는데 이 상태가 **TIME_WAIT**상태 입니다.

3. 서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN플래그를 전송합니다.

4. 클라이언트는 확인했다는 메시지를 보냅니다.

**여기서 TIME_WAIT란?**

- Client에서 세션을 종료시킨 후 뒤늦게 도착하는 패킷으로 인해 데이터가 유실되는 현상이 발생될 수 있습니다.
- 이런 상황을 대비해서 **Client는 Server로부터 FIN을 수신하더라도 일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정**을 가집니다. 이것을 TIME_WAIT라고 합니다.

이렇게, TCP의 초기화와 종료를 담당하는 두 방식에 대해 알아보았습니다.

## TCP Segment

보내거나 종료하는 방식까진 알겠는데 데이터가 어떤 형태로 가는지도 한번 알아봐야 겠죠?

결론적으로 말하자면 저희가 브라우저 렌더링을 할 때 서버로부터 JS, CSS, HTML 파일들을 받을텐데 그 것들은 한 파일 채로 오지 않습니다.

원래 파일을 쪼개서 **패킷으로 분할되서 옵니다!**

또한, 여기서 골치아픈게 이게 좀 뒤죽박죽 순서가 섞여서 옵니다.
=> ?? : 그럼 이걸 나중에 어떻게 조립하죠?

이건, TCP Segment에 정답이 있습니다. TCP Segment를 알아봅시다.

TCP 세그먼트 내에는 우선 여러 개의 구성 요소가 있습니다. 이번 포스트에서 다 다루지 않을 것이고, 가장 중요한 것들 위주로 볼 것입니다.

### TCP Segment의 구성요소

**1. TCP Header**
![image](https://user-images.githubusercontent.com/60723373/174488356-b94411c1-2bb3-4a8e-99c2-b74d47b006e4.png)

꽤 많은 것들이 들어 있으나 중요한 것들만 보겠습니다.

- Source port address : 송신측 주소(from)
- Destination port address : 수신측 주소(to)
- Sequence Number : 담긴 데이터의 순서

**2. Data**
아까 말했듯이 잘린 데이터입니다.

이렇게 두 가지로 TCP Segment는 구성이 됩니다.

또한, 이제 위의 궁금증이 풀립니다. 처음에는 물론 데이터를 완성하기 위해 각 데이터의 순서를 몰랐지만 TCP Header내에 Sequence Number 덕분에 완전한 파일로 만들 수 있었습니다.

또한, 가끔씩 데이터가 하나 빠져서 오는 경우도 있는데 이 때도, Sequnce Number를 통해 판단 후에 빈 데이터를 다시 받을 수 있겠죠?

여기까지가, 이번 TCP에 대한 설명이였습니다. 다음 포스트는 IP에 대한 설명으로 찾아뵙겠습니다. 감사합니다!

## 출처

https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C_%EC%8A%A4%EC%9C%84%ED%8A%B8

https://developer.mozilla.org/ko/docs/Glossary/TCP

https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake

https://velog.io/@seaworld0125/WEB-TCP%EB%9E%80
