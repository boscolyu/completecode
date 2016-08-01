#### Q : top의 정보가 있는 데 뭐가 뭔지 잘 모르겠다.
A : 
```
top - 16:34:00 up 15 days,  1:29,  4 users,  load average: 0.00, 0.00, 0.00
Tasks:  80 total,   1 running,  79 sleeping,   0 stopped,   0 zombie
Cpu0  :  0.3%us,  0.0%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   1914488k total,  1742032k used,   172456k free,   308960k buffers
Swap:        0k total,        0k used,        0k free,  1015704k cached
```
```
top - 시스템 부팅 후 시간, ?, 현재 로그인한 유저, load average값: 1분, 5분, 15분
Tasks: 현재 구동중인 프로세스, 실행중인 프로세스, 실행 대기중인 프로세스, 멈춘 프로셋, 좀비 프로세스
Cpu : usage, sys, nice, idle, wait, ?, ?, ?
Mem : total, used, free, buffers
Swap : totla, used, free, cached
```

* load average
* zombie
* cpu
  * usage 
  * sys
  * nice
  * idle
  * iowait
* memory 
  * used
  * free
  * buffers
  * cached


#### Q: top 정보에 COMMAND 정보도 부족하고 CPU별 점유율 정보도 부족하다.
A: top을 실행한 다음, 1을 누르면 cpu의 세부 정보가 출력된다. c를 누르면 COMMAND의 세부 정보가 출력된다.
```
top - 15:59:48 up 2 days, 17:39,  2 users,  load average: 0.00, 0.04, 0.05
Tasks:  88 total,   1 running,  87 sleeping,   0 stopped,   0 zombie
Cpu0  :  0.0%us,  0.0%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.3%st
Cpu1  :  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   2097336k total,   819508k used,  1277828k free,   255612k buffers
Swap:        0k total,        0k used,        0k free,   321940k cached

  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
  354 www       15   0  4892  536  468 S  0.0  0.0   0:00.00 tail -f access_hangame_mphoto2_sg.140120 access_mobileweb_cgp.140117 access_mobileweb_cgp.140120 error_log.140117 error_mobileweb_cgp.140120 mod_jk.log.140117 mod_jk.log.140120
  644 www       25   0  339m 128m 8748 S  0.0  6.3   0:10.32 /usr/local/jdk6/bin/java -Dprojectname=hangame_mphoto2 -server -Xms16m -Xmx128m -XX:+UseParallelGC -XX:+UseParallelOldGC -XX:+DisableExplicitGC -Dfile.encoding=euc-kr -Dlog4j.d
  659 root      12  -5     0    0    0 S  0.0  0.0   0:00.00 [kmpathd/0]
  660 root      12  -5     0    0    0 S  0.0  0.0   0:00.00 [kmpathd/1]
  661 root      12  -5     0    0    0 S  0.0  0.0   0:00.00 [kmpath_handlerd]
  683 root      10  -5     0    0    0 S  0.0  0.0   0:00.02 [kjournald]
  931 root      18   0  2424  528  232 S  0.0  0.0   0:00.00 /sbin/dhclient -1 -q -lf /var/lib/dhclient/dhclient-eth0.leases -pf /var/run/dhclient-eth0.pid eth0
  965 root      24   0  1932  760  624 S  0.0  0.0   0:00.02 syslogd -m 0
  968 root      15   0  1776  408  340 S  0.0  0.0   0:00.00 klogd -x
  977 root      15   0  2580  376  268 S  0.0  0.0   0:00.03 irqbalance
  992 root      18   0  2572 1152  996 S  0.0  0.1   0:00.87 /bin/bash /usr/sbin/xe-daemon -p /var/run/xe-daemon.pid
 1073 root      18   0  7144 1116  720 S  0.0  0.1   0:00.03 /usr/sbin/sshd
 1083 root      15   0  2852  888  708 S  0.0  0.0   0:00.00 xinetd -stayalive -pidfile /var/run/xinetd.pid
 1094 ntp       15   0  4420 4416 3428 S  0.0  0.2   0:00.00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
 1102 root      15   0  6456 1136  588 S  0.0  0.1   0:00.01 crond
```
