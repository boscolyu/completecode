#### Q: 현재 장비에서 디스크를 얼마나 사용하는 지 궁금하다.
A: iostat 명령을 이용
```
[localhost /home1/user ]# iostat
Linux 2.6.18-308.11.1.el5xen (dev-ochdev705.ncl)        2014년 01월 20일

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.02    0.00    0.03    4.34    0.02   95.58

Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
xvda              1.20         3.06        13.86     724734    3284648
xvda1             0.34         1.21         3.77     285858     892552
xvda2             0.86         1.85        10.09     438604    2392096
```

```
 그냥 iostat만 입력하면 1번만 실행되고 종료되므로 정확한 데이터를 알 수 없음 
 반드시 parameter로 모니터링 간격을 주고 지속적으로 모니터링 해야함.
```

```
[localhost /home1/user ]# iostat 1
// 1초 간격으로 iostat 결과 화면을 뿌려줌
Linux 2.6.18-308.11.1.el5xen (dev-ochdev705.ncl)        2014년 01월 20일

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.02    0.00    0.03    4.34    0.02   95.58

Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
xvda              1.20         3.06        13.86     724734    3284648
xvda1             0.34         1.21         3.77     285858     892552
xvda2             0.86         1.85        10.09     438604    2392096
```

```
 전송량보다 더 세부적인 정보가 궁금한 경우에는 -x 옵션을 사용함
```

```
[localhost /home1/user ]# iostat -x 1
// 1초 간격으로 iostat 결과 화면을 뿌려줌
Linux 2.6.18-308.11.1.el5xen (dev-ochdev705.ncl)        2014년 01월 20일

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.02    0.00    0.03    4.34    0.02   95.58

Device:         rrqm/s   wrqm/s   r/s   w/s   rsec/s   wsec/s avgrq-sz avgqu-sz   await  svctm  %util
xvda              0.02     0.69  0.15  1.04     3.05    13.86    14.10     0.27  222.77  72.61   8.71
xvda1             0.01     0.18  0.04  0.29     1.20     3.76    14.71     0.16  477.82 119.67   4.04
xvda2             0.01     0.51  0.11  0.75     1.85    10.09    13.86     0.11  122.73  77.93   6.71


// avgqu-sz : 디바이스로 요청이 들어왔지만 처리하지 못하고 대기하고 있는 큐의 크기임 (The average queue length of the requests that were issued to the device.)
// avgrq-sz : 디바이스로 들오온 io request 의 평균값 (The average size (in sectors) of the requests that were issued to the device)
// await : 디바이스로 요청을 받아서 처리한 평균 시간 The average time (in milliseconds) for I/O requests issued to the device to be served. This includes the time spent by the requests in queue and the time spent servicing them.
// %util : 디스크 사용률을 보여줌. 100이면 하드 디스크가 full로 동작하고 있는 거임 (Percentage of CPU time during which I/O requests were issued to the device (bandwidth utilization for the device).)
// svctm : IO 요청을 처리하는 평균 시간 (The average service time (in milliseconds) for I/O requests that were issued to the device.)
```

#### Q : 프로세스가 사용하는 파일의 정보를 보고 싶습니다. 프로세스에서 로드해서 사용하고 있는 라이브러리 정보를 알고 싶습니다.
A : lsof  명령을 이용하세요.
```
[localhost /home1/user ]# lsof | grep java
java      16451    www  294r      REG        8,3    88569   21278635 /home1/www/bloc/bloc_10850/repository/bloc_hsp_social/lib/tiles-core-0.2.jar
java      16451    www  295r      REG        8,3   362030   21278548 /home1/www/bloc/bloc_10850/repository/bloc_hsp_social/lib/ibatis2-sqlmap-2.3.0.677-p5.jar
java      16451    www  297r      CHR        1,9                 976 /dev/urandom
java      16451    www  299u     IPv4    2311776                 TCP *:11005 (LISTEN)
java      16451    www  301u     IPv4    2311778                 TCP *:11006 (LISTEN)
java      16451    www  307r     FIFO        0,6             2311796 pipe
java      16451    www  308w     FIFO        0,6             2311796 pipe
java      16451    www  309r     0000       0,11        0    2311797 eventpoll
```
#### Q : 디스크 점유량을 알고 싶어요.
A : df 명령을 쓰시면 됩니다.
```
[localhost /home1/user ]# df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/cciss/c0d0p1    275668440   6009712 269658728   3% /
tmpfs                  6147692         0   6147692   0% /dev/shm
```

#### Q : df를 써도 block 단위로 나오니까 읽기 힘드네요
A : df -h 옵셔을 주고 쓰시면 됩니다.
```
[localhost /home1/user ]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/cciss/c0d0p1     263G  5.8G  258G   3% /
tmpfs                 5.9G     0  5.9G   0% /dev/shm
```
#### Q : df와 du명령으로 확인을 했는데 크기가 달라요
A : 우선 df는 파일 시스템 기준으로 용량을 계산하고, du 는 실제 디렉토리, 파일을 검사해서 계산하기 때문에 계산 방식이 다릅니다.
    파일 시스템에서는 파일을 삭제해도 다른 프로세스에서 참조하고 있다면 reference count를 유지하고 있고 이를 용량 계산에 포함시킵니다.
    lsof 명령으로 삭제된 파일을 참조 하고 있는 프로세스를 종료시키면 될 것 같습니다.
    중요한 서버가 아니면 그냥 재부팅 시키면 깔끔합니다. ^^

#### Q : raid0, raid1, raid5, raid10 이거 뭔가요?
A : 디스크를 개별로 사용하는 것이 아니라 여러개의 디스크를 묶어서 사용하는 것입니다.
|| raid name || 형태 || 장단점 || 
| raid0 | * 여러개의 디스크를 묶어서 하나로 사용함. | * 디스크 중 하나가 깨지면 데이터 복구 불가능 
 * 성능은 좋음 | 
| raid1 | * 같은 데이터를 2벌로 복제해서 사용함. | * 디스크 중 하나가 깨져도 복구 가능함. 
* 다른 raid에서 2개가 깨지면 복구 불가능. 
* 2번 써야 하므로 속도가 느림| 
| raid5 | * 여러개의 디스크를 묶어서 하나로 사용함. 
* 패리티 비트를 보관하는 영역만큼 사용가능한 데이터 크기 줄어듦 | * 디스크에 parity bit가 포함되어 있어서 디스크 중 하나가 깨지면 복구 가능함. 
* 2개 깨지면 전체 데이터 복구 불가능.  |
| raid10 | * raid1로 묶은 것을 다시 raid0로 묶은 형태임 | * 안정성 최고임
* 사용 가능한 디스크 용량이 절반으로 줄어듦 | 

* 참고 문서 
** http://en.wikipedia.org/wiki/Standard_RAID_levels
** http://en.wikipedia.org/wiki/Nested_RAID_levels