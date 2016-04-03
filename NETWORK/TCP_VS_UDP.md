# UDP vs TCP

원문 : http://gafferongames.com/networking-for-game-programmers/udp-vs-tcp/

## Introduction
안녕하세요. 저는 Glenn Fiedler입니다. 저의 게임 개발자를 위한 Networking 시리즈의 첫번째 문서에 오신 것을 환영합니다.

여기서 우리는 데이터를 주고 받는 기본적인 네트워크 개발에 대해 알아보려고 합니다. 이것은 네트워크 개발자가 알아야 하는 가장 단순하면서도 기본적인 내용이고 시작에 불과합니다. 하지만 내용은 꽤 복잡하고 어떤 것을 해야할 지 명확해 보이지 않습니다. 만약 네트워크 개발에 대해서 잘못 이해하고 있다면, 당신의 multiplayer 게임에 심각한 악영향을 끼칠 수 있으니 주의해야 합니다.

우리들은 대부분 소켓이라는 용어에 익숙합니다. 그리고 TCP와 UDP 두 개 타입의 소켓이 있다는 것도 알고 있습니다. 실제 우리가 네트워크 게임을 개발할 때는 어떤 타입의 소켓을 사용할 지 결정해야 합니다. TCP를 사용할까요? UDP를 사용할까요? 아니면 섞어서?

소켓 타입의 결정은 전적으로 당신이 어떤 게임을 개발하는 냐에 달려 있습니다. 이 글에서든 이후 다른 글에서든 저는 당신이 Action 게임 개발자라고 생각하고 글을 쓸 것입니다. 당신이 Halo, Battlefield 1942, Quake, Unreal, CounterStrike, Team Fortress와 같은 게임은 잘 알고 있다고 생각하고 글을 쓸 예정입니다.

우리는 Action 게임 네트워크 중에서 각 소켓 별 세부 속성에 대해서 먼저 살펴보려고 합니다. 그리고 어떻게 인터넷이 동작하는 지에 대해 살펴볼 예정입니다. 우리가 이 모든 것을 이해한다면, 정확한 결정을 할 수 있을 것입니다.

## TCP/IP

TCP는 “Transmission control protocol”의 약자이고 IP는 “Internet protocol”의 약자입니다. TCP와 IP는 당신이 온라인에서 하는 웹서핑, IRC, 이메일 등 거의 대부분의 일들이 이 TCP/IP를 기반 위에서 동작합니다. 

만약 한 번이라도 TCP 소켓을 사용한 경험이 있다면, 당신은 TCP 연결이 프로토콜을 기반으로 한 매우 신뢰성있는 연결임을 알고 있습니다. TCP는 2개의 장비 사이에 Connection을 만들고 한쪽 장비에서 파일을 쓰면, 다른 쪽 장비에서 파일을 읽어들일 수 있습니다. 

이 TCP Connection은 높은 신뢰성에다 순서도 꼬이지 않습니다. TCP는 당신이 한 쪽 장비에서 보낸 순서대로 반대편 장비에서도 데이터를 읽을 수 있도록 보장해줍니다.

정리하면 TCP로 데이터를 주고 받는 것은 파일을 쓰는 것과 비슷합니다. 간단하죠!

## IP

TCP 프로토콜 바로 아래에서 동작하는 IP 프로토콜은 TCP에 비해서 훨씬 더 간단합니다. 

여기에는 Connection의 개념이 없습니다. 대신에 패킷들은 컴퓨터에서 다음으로 이동합니다. 쉬운 예로 설명하자면, 당신이 직접 손으로 쓴 노트를 복잡한 방을 가로질러 반대편으로 전달하는 것입니다. 수많은 사람들의 손을 거쳐서 노트는 방의 반대편 사람에게 전달됩니다.

당신이 작성한 노트가 반드시 전달하려는 사람에게 전달된다는 보장은 없습니다. 작성자는 노트를 전달하면서 잘 보내지기를 바랄 뿐입니다. 상대방이 답장을 하기로 결정했더라도, 노트가 잘 도착했는 지 알 수가 없습니다.

물론 실제 패킷의 전달은 이것보다 훨씬 복잡합니다. 어떤 컴퓨터도 패킷을 빨리 도착지에 보낼 수 있도록 전달은 하지만 전달하는 정확한 순서를 알 수는 없습니다. 어떤 경우에는 같은 패킷을 여러 번 보낼 수도 있습니다. 패킷들은 여러 경로를 통해서 목적지에 전달되고, 도착 시간도 패킷마다 각각 다릅니다.  

이것은 인터넷 프로토콜이 일부 커넥션에 문제가 생기더라도 패킷을 전달할 수 있도록 자체적으로 조직되고, 복구되도록 만들어졌기 때문입니다. 

## UDP

컴퓨터간의 통신을 파일간 통신처럼 전달하는 것외에 다른 방법으로 패킷을 직접 전달하는 방법은 없을까요? 

우리는 UDP를 이용해서 이런 작업을 할 수 있습니다. UDP는 “User datagram protocol”의 약자로 TCP처럼 IP프로토콜 위에서 동작하고 있습니다. UDP 프로토콜은 많은 복잡다단한 특징과 복잡성을 추가하는 대신에  IP 프로토콜위에 가장 단순한 구조로 올라가 있습니다. 

UDP를 통해서 우리는 패킷을 목적지 주소로 전달할 수 있습니다. 112.140.20.10:52423 주소로 패킷을 전달할 수 있는 것입니다. 

수신측에서 우리는 특정 포트를 열고 기다리고 있습니다. 그리고 특정 컴퓨터에서 패킷이 도착하면 우리는 패킷을 전송한 송신측의 주소와 포트 정보 데이터의 길이를 알 수 있고, 데이터를 읽어들일 수 있습니다.

UDP는 신뢰성을 보장하는 프로토콜이 아닙니다. 예를 들면, 대부분의 패킷들은 잘 받을 수 있지만, 1 ~ 5%의 패킷은 중간에 유실됩니다. 그리고 경우에 따라서는 전혀 패킷을 받지 못하는 경우가 발생할 수도 있습니다.  (당신이 통신을 원하는 컴퓨터와 컴퓨터 사이에는 잘못을 저질를 수 있는 수많은 컴퓨터가 있다는 것을 명심하세요.)

패킷의 순서를 보장할 수 없습니다. 당신의 1,2,3,4,5의 순서로 5개의 패킷을 보내지만, 목적지에서는 3, 1, 2, 5, 4의 순서로 받을 수 있습니다. 실제적으로는 거의 대부분의 패킷들은 제대로 된 순서로 패킷을 수신합니다. 하지만 거기에 의존하면 안됩니다.

결론적으로, UDP는 IP계층 위에서 많은 일을 하고 있지만 한가지 보장해주는 것이 있습니다. 만약 당신이 패킷을 보냈을 경우에 패킷 전체가 전송되거나 실패하는 것이다. 일부만 전송에 성공하는 경우는 없습니다. 그래서 만약 당신이 256byte를 목적지로 전송한다면, 목적지 호스트에서는 100byte만 수신하는 경우는 발생하지 않습니다. 256byte전체를 수신합니다. UDP는 단지 이것만을 보장하며, 나머지는 모두 당신의 몫입니다.  

## TCP vs UDP

우리는 여기서 TCP 소켓을 사용할 것인지, UDP 소켓을 사용할 것인지 결정해야 합니다.
각 프로토콜별 특징을 살펴보겠습니다.

TCP:   
     Connection 기반   
     신뢰성 있는 연결과 순서를 보장함.  
     자동으로 당신의 데이터를 패킷으로 바꾸어서 전달함.   
     패킷을 전송하는 속도를 조절할 수 있다. (flow control)   
     쉽게 사용할 수 있다. 파일에 쓰고 읽듯이 사용하면 됨.   
UDP:   
     Connection 기반의 연결이 아님. 모두 자체적으로 소화해야 함.   
     신뢰성 있는 결과 순서를 보장하지 않음. 중복이 발생할 수도 데이터가 유실 될 수도 있음.   
     수동으로 당신의 데이터를 패킷으로 바꾸어서 전송하는 작업을 해야 함.   
     패킷을 전송하는 속도를 인터넷 환경을 위해서 너무 빨리 전송하지 않도록 적절하게 조절해야 함.    
     패킷이 유실되면, 유실을 감지할 수 있는 방법은 고안해야 하고, 경우에 따라서는 재전송해야 함.   

결정하기가 이전보다는 조금 명확해졌습니다. TCP는 우리가 원하는 모든 데이터를 전달하고 사용하기가 편합니다. 반면에 UDP는 작업하기가 어렵고 모든 것을 직접 작성해야 합니다. 그래서 우리는 TCP를 사용하는 것이 맞을까요?


Wrong.

하지만 만약 당신이 FPS와 같은 네트워크 액션 게임을 개발하는 경우라면 TCP를 사용하는 것은 가장 나쁜 선택이 될 가능성이 높습니다. 그러기 위해서 우리는 간단하게 보이는 TCP가 실제로 IP 계층 위해서 하는 모든 것을 알 필요가 있다. 

## How TCP really works

TCP와 UDP는 모두 IP 계층 위에 존재합니다. 그러나 둘은 완전히 다릅니다. UDP는 바로 밑에 있는 IP 계층처럼 동작하지만, 모든 것을 추상화시킨 TCP는 패킷의 모든 복잡함과 비신뢰성을 가려서, 파일을 읽고 쓰는 것처럼 간단해 보입니다. 

So how does it do this?

우선 TCP는 stream 프로토콜입니다. 그래서 당신은 스트림에 데이터를 쓰고, TCP는 이 데이터를 목적지로 잘 전송합니다. IP는 패킷을 기반으로 하고 TCP 는 IP를 기반으로 하기 때문에 TCP는 당신의 데이터를 패킷으로 분할시킵니다. 그래서 어떤 내부 TCP code들은 데이터를 큐잉하고 충분한 데이터가 쌓인 이후에야 목적지로 패킷을 전송합니다.

이것은 multiplayer 게임에서 문제를 일으킵니다. 만약 당신이 매우 작은 크기의 패킷을 전송한다면, TCP는 당신이 보내는 데이터가 충분히 쌓일 때까지 패킷을 전송하지 않고 기다리는 현상이 발생하게 됩니다. (100 byte정도의 데이터 이상이 될때까지) 당신은 클라이언트 유저들이 가능한 한 최대한 빨리 서버로 데이터를 보낼 수 있기를 원하지만 실제로는 그럴 수가 없습니다. 만약 당신이 TCP 처럼 데이터 전송을 지연시키거나 쌓아두기만 한다면, 멀티플레이어 게임에서의 사용자 경험은 매우 떨어집니다. 게임 네트워크 입장에서 TCP는 우리가 원하는 시점에 제대로 전송하지 못하고 때때로 늦어집니다.

TCP에는 TCP_NODELAY라고 부르는 옵션을 사용해서 이런 점들을 보완할 수 있습니다. 이 옵션은 충분한 데이터가 쌓이기 전에 데이터를 전송하라고 명령을 내립니다. but to send whatever data you write to it immediately. This is typically referred to as disabling Nagle’s algorithm.

불운하게도 당신이 TCP에 이 옵션을 활성화시킨다 하더라도 멀티 플레이어 게임에는 문제를 일으킵니다.

그것은 TCP 가 패킷의 유실이나 순서의 뒤바뀜을 처리하기 위해서 만들어진 것이기 때문입니다. 그것은 당신에게 데이터가 신뢰성있고 제대로 된 순서의 데이터라는 "illusion"을 선물하기 위한 것입니다.

## How TCP implements reliability.

기본적으로 TCP는 Stream의 데이터를 패킷으로 쪼개어서 패킷을 신뢰할 수 없는 IP를 이용하여 보냅니다. 그러면 패킷을 받은 반대편에서는 stream을 재구성합니다. 

하지만, 패킷을 잃어버렸을 때는 어떻게 할까요? 패킷이 중복되거나 순서대로 전달되지 않은 경우에는 어떻게 할까요?

여기서 TCP의 세부적인 내용은 매우 복잡하기 때문에 깊이 들어가지는 않을 것입니다. (만약 원한다면 TCP/IP illustrated를 참조하세요)  기본적으로 TCP는 패킷을 전송하고 패킷이 손실없이 잘 전송될 때까지 일정 시간 기다립니다. 기다리는 시간동안 ack(acknowledgement) 메시지를 받지 못하면 패킷이 유실되었다고 판단하고 다시 패킷을 재전송합니다. 중복된 패킷은 받는 쪽에서 버립니다. 그리고 순서가 잘못된 패킷은 제대로 된 데이터로 만들기 위해서 재배열시킵니다. 

문제는 만약 우리들이 TCP를 이용하여 데이터를 전송하기를 원할 경우 패킷이 유실될 경우 멈추고 재전송될 때까지 기다립니다. 더 최신의 패킷이 이미 도착해 큐에 쌓여있더라도 이전의 유실된 패킷이 도착할 때까지 접근할 수가 없습니다. 패킷이 재전송될 때까지는 얼마나 걸릴까요? 데이터를 재전송하는 데는 적어도 RTT(Round Trip Time)의 시간이 걸릴 것입니다. 

## Why you should never use TCP to network time critical data.

FPS와 같은 real time game을 TCP를 이용해서 개발할 경우의 문제점은 웹브라우저, 이메일 또는 다른 대부분의 어플리케이션과는 다릅니다. 이런 multiplayer game은 실시간을 패킷을 전송해야하는 요구사항이 있습니다. 예를 들어 사용자의 입력이나 캐릭터의 위치같은 당신의 게임의 대부분의 정보들은 몇 초전의 데이터들은 의미가 없습니다. 당신은 단지 최신의 데이터에 대해서만 관심을 가질 뿐입니다. TCP는 이런 것을 위해서 디자인된 프로토콜이 아닙니다. 

FPS 멀티 플레이어 게임의 가장 단순한 예제를 생각해보죠. 당신은 가장 단순한 방법으로 데이터를 전송하기를 원합니다. 키입력, 마우스 입력과 같은 클라이언트의 프레임 정보는 모두 서버로 전송하구요. 서버는 클라이언트로부터 받은 개별 플레이어의 입력을 처리하여 클라이언트가 렌더링해야할 game object의 현재 위치를 전달합니다.

그래서 우리의 단순한 멀티 플레이어 게임은 패킷이 유실될 때마다 모든 것은 멈추고 패킷이 재전송될 때까지 기다립니다. 클라이언트 게임은 업데이트 받는 것을 멈추고 그 자리에 그대로 서있을 것이고, 서버는 클라이언트로부터 입력을 받지 못해서 캐릭터는 움직이거나 쏠 수가 없습니다. 패킷이 마침내 도착하지만, 당신은 시간이 지나간 이 정보들에 대해서 신경쓰고 싶지 않을 것입니다. 

## Wait? Why can’t I use both UDP and TCP?

For realtime game data like player input and state, only the most recent data is relevant, but for other types of data, say perhaps a sequence of commands sent from one machine to another, reliability and ordering can be very important.

The temptation then is to use UDP for player input and state, and TCP for the reliable ordered data. If you’re sharp you’ve probably even worked out that you may have multiple “streams” of reliable ordered commands, maybe one about level loading, and another about AI. Perhaps you think to yourself, “Well, I’d really not want AI commands to stall out if a packet is lost containing a level loading command – they are completely unrelated!”. You are right, so you may be tempted to create one TCP socket for each stream of commands.

On the surface, this seems like a great idea. The problem is that since TCP and UDP are both built on top of IP, the underlying packets sent by each protocol will affect each other. Exactly how they affect each other is quite complicated and relates to how TCP performs reliability and flow control, but fundamentally you should remember that TCP tends to induce packet loss in UDP packets. For more information, read this paper on the subject.

Also, it’s pretty complicated to mix UDP and TCP. If you mix UDP and TCP you lose a certain amount of control. Maybe you can implement reliability in a more efficient way that TCP does, better suited to your needs? Even if you need reliable-ordered data, it’s possible, provided that data is small relative to the available bandwidth to get that data across faster and more reliably that it would if you sent it over TCP. Plus, if you have to do NAT to enable home internet connections to talk to each other, having to do this NAT once for UDP and once for TCP (not even sure if this is possible…) is kind of painful.


## Conclusion

My recommendation then is not only that you use UDP, but that you only use UDP for your game protocol. Don’t mix TCP and UDP, instead learn how to implement the specific pieces of TCP that you wish to use inside your own custom UDP based protocol.

Of course, it is no problem to use HTTP to talk to some RESTful services while your game is running — that’s not what I’m saying. A couple TCP connections running while your game is running isn’t going to bring everything down. The point is, don’t split your game protocol across UDP and TCP. Keep your game protocol running over UDP so you are fully in control of the data you send and receive and how reliability, ordering and congestion avoidance are implemented.

The rest of the articles in this series show you how to do this, from creating your own virtual connection based protocol on top of UDP, to creating your own reliability, flow control and congestion avoidance over UDP.
