#### Q : 도메인에 대한 ip 정보를 알고 싶다.
A : nslookup 명령을 이용해서 확인이 가능합니다.

```
[localhost:/home1/user] nslookup toast.com
Server:         127.0.0.1
Address:        127.0.0.1#53

Non-authoritative answer:
Name:   toast.com
Address: 183.110.213.121

```

#### Q : 현재 네트워크 사용 정보를 확인하고 싶다. listen으로 열려 있는 포트 정보들을 확인하고 싶다.
A : netstat 명령으로 확인 가능함

```
[localhost:/home1/user] netstat | grep tcp
tcp        0      0 boscolyu-HP-Elite:56101 nrt19s12-in-f2.1e:https ESTABLISHED
tcp        0      0 boscolyu-HP-Elite:55200 tb-in-f125.:xmpp-client ESTABLISHED
tcp        1      0 boscolyu-HP-Elite:34847 mulberry.canonical:http CLOSE_WAIT 
[localhost:/home1/user] netstat -na | grep tcp
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN     
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN     
tcp        0      0 10.77.38.192:56101      74.125.235.162:443      ESTABLISHED
tcp        0      0 10.77.38.192:55200      74.125.31.125:5222      ESTABLISHED
tcp        1      0 10.77.38.192:34847      91.189.94.25:80         CLOSE_WAIT 
tcp        0      0 10.77.38.192:55214      10.24.48.47:80          ESTABLISHED
tcp6       0      0 ::1:631                 :::*                    LISTEN 
```

#### Q: 윈도우의 ipconfig와 비슷한 명령은?
A: ifconfig 명령을 쓰면 됩니다.

```
[localhost:/home1/user] /sbin/ifconfig
eth0      Link encap:Ethernet  HWaddr 2c:41:38:01:a2:94  
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
          Interrupt:20 Memory:d4700000-d4720000 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:5133 errors:0 dropped:0 overruns:0 frame:0
          TX packets:5133 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:395187 (395.1 KB)  TX bytes:395187 (395.1 KB)
```

#### Q: 웹에 있는 파일을 다운로드 하고 싶습니다.
A: wget 명령을 사용하시면 됩니다.

```
## m.naver.com의 내용을 target.html로 저장함
[localhost:/home1/user] wget -O target.html "http://m.naver.com"

## http request, response의 헤더 정보를 보고 싶은 경우에는 -d 옵션을 사용한다.
[localhost:/home1/user] wget -d -O target.html "http://m.naver.com"

## wget하면서 로그 정보를 별도 파일로 저장하고 싶은 경우에는  -o옵션을 사용한다.
[localhost:/home1/user] wget -d -o log.log -O target.html "http://m.naver.com"

## proxy를 사용하지 않고 받고 싶은 경우에는 --no-proxy 옵션을 사용한다.
[localhost:/home1/user] wget -d --no-proxy  -O target.html "http://m.naver.com"

## proxy를 사용하지 않고 받고 싶은 경우에는 --no-proxy 옵션을 사용한다.
[localhost:/home1/user] wget --header='Accept-Charset: iso-8859-2' \
--header='Accept-Language: hr'        \
-O target.html "http://m.naver.com"

## 요청시의 user-agent를 설정해서 보내고 싶다.
[localhost:/home1/user] wget --user-agent='google' -O target.html "http://m.naver.com

## post로 데이터를 전송하고 싶은 경우
```
    
#### Q: ssh 접속을 할 때 비밀번호를 물어보지 않게 하고 싶다.
A: 키 교환을 해주면 물어보지 않습니다.

#### Q: 리눅스에서 장비간  파일 전송을 어떻게 하나요?
A: 일반적으로는 rsync 명령을 이용하실 수 있습니다.

#### Q: 목적지 IP까지 경유하는 route 정보와 시간을 보고 싶습니다. 
A : traceroute 명령을 사용하시면 됩니다.

```
[localhost:/home1/user] traceroute 10.99.208.96
traceroute to 10.99.208.96 (10.99.208.96), 30 hops max, 40 byte packets
 1  10.101.28.2 (10.101.28.2)  1.344 ms  1.471 ms  2.173 ms
 2  10.22.44.113 (10.22.44.113)  0.796 ms  0.788 ms  0.776 ms
 3  10.22.46.222 (10.22.46.222)  1.221 ms  1.219 ms 10.22.46.226 (10.22.46.226)  1.187 ms
 4  10.99.208.96 (10.99.208.96)  1.956 ms  1.951 ms  1.939 ms
```