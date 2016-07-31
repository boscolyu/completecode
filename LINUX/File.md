Q: 파일의 마지막 부분을 보고 싶을 경우에는?
A: tail 명령을 이용한다.

{code}
// 마지막 100라인의 내용을 보고 싶은 경우
[localhost:/home1/user] tail -n 100 linuxFile.txt
{code}

Q: 파일의 앞부분을 보고 싶을 경우에는?
A: head 명령을 이용한다.

{code}
// 마지막 100라인의 내용을 보고 싶은 경우
[localhost:/home1/user] head -n 100 linuxFile.txt
{code}


Q: 파일에 계속 쌓이는 로그들을 보고 싶을 경우에는"
A: tail \-f 로 실행한다.

{code}
[localhost:/home1/user] tail -f windowFile.txt
{code}

Q: 파일 여러개를 한꺼번에 보고 싶다.
A: tail \-f 에 디렉토리를 붙이거나 패턴으로 파일을 지정하면 해당 디렉토리 또는 파일들이 모두 출력됨

{code}
[localhost:/home/www/backup/bloclog] tail -f ./*

==> ./bloc_5810-lucy.2014-03-06.log <==
2014. 3. 6 오후 2:50:48 com.nhncorp.lucy.bloc.tracer.StatusLogAppender append
정보: [SRTR1000I] Status [{run=0,slow=0,ips=0},{jvm=2.96%,os=15.50%},{NPC=0/256,HTTP=0/5000,MRSREXDR=0/100}]
2014. 3. 6 오후 2:50:58 com.nhncorp.lucy.bloc.tracer.StatusLogAppender append
정보: [SRTR1000I] Status [{run=0,slow=0,ips=0},{jvm=2.96%,os=15.50%},{NPC=0/256,HTTP=0/5000,MRSREXDR=0/100}]
2014. 3. 6 오후 2:51:08 com.nhncorp.lucy.bloc.tracer.StatusLogAppender append
정보: [SRTR1000I] Status [{run=0,slow=0,ips=0},{jvm=2.99%,os=15.50%},{NPC=0/256,HTTP=0/5000,MRSREXDR=0/100}]
2014. 3. 6 오후 2:51:18 com.nhncorp.lucy.bloc.tracer.StatusLogAppender append
정보: [SRTR1000I] Status [{run=0,slow=0,ips=0},{jvm=2.99%,os=15.50%},{NPC=0/256,HTTP=0/5000,MRSREXDR=0/100}]
2014. 3. 6 오후 2:51:28 com.nhncorp.lucy.bloc.tracer.StatusLogAppender append
정보: [SRTR1000I] Status [{run=0,slow=0,ips=0},{jvm=2.99%,os=15.50%},{NPC=0/256,HTTP=0/5000,MRSREXDR=0/100}]
==> ./bloc_5810-lucy.2014-03-06.log <==
2014. 3. 6 오후 2:51:38 com.nhncorp.lucy.bloc.tracer.StatusLogAppender append
정보: [SRTR1000I] Status [{run=0,slow=0,ips=0},{jvm=2.99%,os=15.50%},{NPC=0/256,HTTP=0/5000,MRSREXDR=0/100}]
==> ./bloc_5810-catalina.2014-03-06.log <==
2014. 3. 6 오후 2:51:41 com.nhncorp.mrs.ControlMessageHandler hello
정보: HELLO message sent; state=ACTIVATED
2014. 3. 6 오후 2:51:43 com.nhncorp.mrs.ControlMessageHandler hello
{code}

Q: 윈도우의 파일을 리눅스로 옮겼더니 carrage return 값이 이상하다고 한다.
A: vi로 파일을 연 다음 포맷을 변경한다.

{code}
[localhost:/home1/user] ./run.sh
-bash: ./run.sh: /bin/bash^M: bad interpreter: 그런 파일이나 디렉터리가 없습니다
[localhost:/home1/user] vi run.sh
// command mode에서 입력
:set ff=unix
{code}

{code}
dos2unix, unix2dos 명령도 있음
{code}

Q: vi로 파일을 열었는 데 너무 커서 로딩에 올래 걸린다. 중간에 로딩을 멈추고 싶은 경우
A: vi로 파일을 연 다음 ctrl + C : 로딩 도중에 누르면 중간에 멈춤

Q: 콘솔창에서 vi로 파일을 열어서 윈도우의 텍스트를 붙여넣었더니 탭이 계속 먹으면서 옆으로 쭉 펼쳐져서 이상하게 보인다.
A: set paste를 입력해준다.

{code}
[localhost:/home1/user] vi pasteText.txt
// command mode에서 입력
:set paste
{code}


Q: vi에서 파일의 맨 마지막으로 이동하고 싶다.
A: command mode에서 shift + g 키를 누른다.

Q: vi에서 파일의 맨 첫라인으로 이동하고 싶다.
A: command mode에서 1 키를 누르고, shift + g 키를 누른다.

Q: vi에서 특정 문자열을 찾고 싶다.
A: command mode에서 "/"를 누르고 찾고 싶은 문자열을 입력하고 엔터를 친다. n키를 입력하면 다음 매치되는 곳으로 이동한다.

Q: 파일에서 모든 previous 문자열을 after 문자열로 바꾸고 싶다.
A: vi로 파일을 열고 command mode에서 아래와 같이 입력한다. 정규식도 지원한다.

{code}
:%s /previous/after/g
{code}


Q: 내용을 고칠 것은 아니지만 큰 파일을 읽기 용도로 열어보고 싶다. vi는 너무 오래걸린다.
A: more 명령을 이용해서 파일을 열어본다.

{code}
[localhost:/home1/user] more openBigFileForReading.txt
## f 키를 누르면 page down, b 키를 누르면 page up임
## 끝내고 싶으면 q를 입력한다.
{code}

Q: 파일의 내용을 전부 출력하고 싶다.
A: cat 명령을 이용해서 파일을 열어본다.

{code}
[localhost:/home1/user] cat displayAllText.txt
{code}

Q: 디렉토리 구조가 궁금하다.
A: tree 명령으로 확인한다.

{code}
[localhost /home/www/backup]# tree
.
|-- apachelog
|-- bloclog
|   |-- bloc_10850.out
|   |-- bloc_10850.out.2014.03.05
|   |-- bloc_5810-access.2014-02-25.log
|   |-- bloc_5810-access.2014-03-05.log
|   |-- bloc_5810-admin.2014-02-25.log
|   |-- bloc_5810-admin.2014-03-05.log
|   |-- bloc_5810-catalina.2014-02-25.log
|   |-- bloc_5810-catalina.2014-03-05.log
|   |-- bloc_5810-catalina.2014-03-06.log
|   |-- bloc_5810-host-manager.2014-02-25.log
|   |-- bloc_5810-host-manager.2014-03-05.log
|   |-- bloc_5810-localhost.2014-02-25.log
|   |-- bloc_5810-localhost.2014-03-05.log
|   |-- bloc_5810-lucy.2014-02-25.log
|   |-- bloc_5810-lucy.2014-03-05.log
|   |-- bloc_5810-lucy.2014-03-06.log
|   |-- bloc_5810-manager.2014-02-25.log
|   |-- bloc_5810-manager.2014-03-05.log
|   `-- heapdump
`-- tomcatlog
{code}

Q: tree 명령에 사이즈도 같이 보고 싶다.
A: tree \-s 를 입력한다.

{code}
[localhost /home/www/backup]# tree -s
.
|-- [     4096]  apachelog
|-- [     4096]  bloclog
|   |-- [   211120]  bloc_10850.out
|   |-- [    61972]  bloc_10850.out.2014.03.05
|   |-- [        0]  bloc_5810-access.2014-02-25.log
|   |-- [        0]  bloc_5810-access.2014-03-05.log
|   |-- [        0]  bloc_5810-admin.2014-02-25.log
|   |-- [        0]  bloc_5810-admin.2014-03-05.log
|   |-- [    15912]  bloc_5810-catalina.2014-02-25.log
|   |-- [   509697]  bloc_5810-catalina.2014-03-05.log
|   |-- [  2979946]  bloc_5810-catalina.2014-03-06.log
|   |-- [        0]  bloc_5810-host-manager.2014-02-25.log
|   |-- [        0]  bloc_5810-host-manager.2014-03-05.log
|   |-- [        0]  bloc_5810-localhost.2014-02-25.log
|   |-- [        0]  bloc_5810-localhost.2014-03-05.log
|   |-- [    25187]  bloc_5810-lucy.2014-02-25.log
|   |-- [   174128]  bloc_5810-lucy.2014-03-05.log
|   |-- [  1012362]  bloc_5810-lucy.2014-03-06.log
|   |-- [        0]  bloc_5810-manager.2014-02-25.log
|   |-- [        0]  bloc_5810-manager.2014-03-05.log
|   `-- [     4096]  heapdump
`-- [     4096]  tomcatlog
{code}

Q: 데이터가 존재하는 큰 파일을 여러개의 파일로 쪼개고 싶다.
A: split 명령을 이용한다.

{code}
## 파일을 5000라인 단위로 쪼개고 쪼개지는 파일들은 모두 앞에 prefix_를 붙이고 싶다.
[localhost /home/www/backup/bloclog]# split -l 5000 bloc_5810-lucy.2014-03-06.log prefix_
[localhost /home/www/backup/bloclog]# ls -al prefix_a*
-rw-rw-r-- 1 www www 467860  3월  6 15:04 prefix_aa
-rw-rw-r-- 1 www www 468580  3월  6 15:04 prefix_ab
-rw-rw-r-- 1 www www  79662  3월  6 15:04 prefix_ac
{code}

Q: 파일을 하나 생성하고 싶다.
A: touch 명령을 이용한다.

{code}
[localhost /home1/user]# touch prefix_ad
{code}

Q: 2개의 파일을 비교해서 차이점을 보고 싶다.
A: diff 명령을 이용한다.

{code}
[localhost /home1/user]# diff prefix_aa prefix_ab
{code}


Q: 특정 이름을 가진 파일을 찾고 싶다.
A: find 명령을 이용한다.

{code}
## 현재 디렉토리 밑으로 patch라는 단어가 앞에 붙은 파일을 찾고 싶다.
[localhost /home/user]# find ./ -name "patch*"
./script/install_www/gameplatform/upgrade_bloc_library/patch_server.sh
./script/install_www/gameplatform/upgrade_bloc_library/patch_server_onestop.sh
./script/install_www/gameplatform/upgrade_bloc_library/patch_profile_servers.sh
./apps/apache-ant-1.7.1/docs/manual/CoreTasks/patch.html
./patch_server_hspapp-ami701.log
{code}

{code}
## 찾은 파일을 지우고 싶다.
[localhost /home/user]# find ./ -name "patch_server_hspapp-ami701*"
./patch_server_hspapp-ami701.log
[localhost /home/user]# find ./ -name "patch_server_hspapp-ami701*" -exec rm -rf {} \;


## 현재 디렉토리 밑으로 patch라는 단어가 앞에 붙은 파일을 찾고 싶다.
[localhost /home/user]# find ./ -name "patch*"
./script/install_www/gameplatform/upgrade_bloc_library/patch_server.sh
./script/install_www/gameplatform/upgrade_bloc_library/patch_server_onestop.sh
./script/install_www/gameplatform/upgrade_bloc_library/patch_profile_servers.sh
./apps/apache-ant-1.7.1/docs/manual/CoreTasks/patch.html
./patch_server_hspapp-ami701.log
{code}

{code}
## 마지막 수정일이 1일 이내의 파일을 찾고 싶다.
[localhost /home/user]# find ./* -mtime -1
./bloc_10850.out
./bloc_10850.out.2014.03.05
./bloc_5810-access.2014-03-05.log
./bloc_5810-admin.2014-03-05.log
./bloc_5810-catalina.2014-03-05.log
./bloc_5810-catalina.2014-03-06.log
./bloc_5810-host-manager.2014-03-05.log
./bloc_5810-localhost.2014-03-05.log
./bloc_5810-lucy.2014-03-05.log
./bloc_5810-lucy.2014-03-06.log
./bloc_5810-manager.2014-03-05.log


## 마지막 수정일이 7일 이상된의 파일을 찾고 싶다.
[localhost /home/user]# find ./* -mtime +7
./bloc_5810-access.2014-02-25.log
./bloc_5810-admin.2014-02-25.log
./bloc_5810-catalina.2014-02-25.log
./bloc_5810-host-manager.2014-02-25.log
./bloc_5810-localhost.2014-02-25.log
./bloc_5810-lucy.2014-02-25.log
./bloc_5810-manager.2014-02-25.log


## 마지막 수정일이 7일 이상된의 파일을 찾아서 지우고 싶다.
[localhost /home/user]# find ./* -mtime +7 -exec rm -rf {} \;
{code}


Q: 파일, 디렉토리, 스트림에서 특정 텍스트를 찾고 싶다.
A: grep 명령을 이용한다.

{code}
## 로그 파일에서 "ERROR" 라는 키워드가 들어간 파일의 이름과 라인 정보, 내용을 보고싶다.
[localhost /home/user]# grep -n "ERROR" ./*
./bloc_10850.out.2014.03.05:29:2014-02-25 16:55:24 [ERROR](Gateway.java:63) [Gateway] Failed to connect CM. dev.xnbasearc.navercorp.com:2181 - hsp_social_game_qa
./bloc_10850.out.2014.03.05:81:2014-02-25 16:55:24 [ERROR](ContextLoader.java:227) Context initialization failed
{code}

Q: grep에서 주위 텍스트들도 같이 보고 싶다.
A: -A, -B, -C 옵션을 사용한다.

{code}
## 뒷부분 3라인을 보고 싶다.
[localhost /home/user]# cat srs.log | tail -n 20 | grep -A 3 88899626759592299
## 앞부분 3라인을 보고 싶다.
[localhost /home/user]# cat srs.log | tail -n 20 | grep -B 3 88899626759592299
## 앞, 뒷부분 3라인을 보고 싶다.
[localhost /home/user]# cat srs.log | tail -n 20 | grep -C 3 88899626759592299
{code}

Q: grep에서 특정 확장자만 검색하고 싶다.
A: --include 옵션을 사용한다.

{code}
## cpp 파일 중 SEARCH_ME 라는 키워드를 검색
[localhost /home/user]# grep --include="*.cpp" "SEARCH_ME" .
## 하위디랙토리까지 몽땅 검색
[localhost /home/user]# grep -R --include="*.cpp" "SEARCH_ME" .
{code}

Q: 파일 또는 스트림을 정렬하고 싶다.
A: sort 명령을 이용한다.
{code}
## 파일에서 INFO가 들어간 내용을 출력하되 두번째 컬럼인 시간순으로 출력하고 싶다.
[localhost /home/user]# grep -n "INFO" bloc_10850.out.2014.03.05 | sort -k 2

227:2014-03-05 21:30:07 [INFO ](PlugInLoader.java:80) 다음 플러그인 로딩을 시작합니다. scheduler=com.nhncorp.lucy.common.plugin.component.SchedulerPlugIn
224:2014-03-05 21:30:07 [INFO ](PlugInLoader.java:80) 다음 플러그인 로딩을 시작합니다. sqlMap=com.nhncorp.lucy.db.SqlMapPlugIn
228:2014-03-05 21:30:07 [INFO ](SchedulerPlugIn.java:59) Scheduler initialized
235:2014-03-05 21:30:07 [INFO ](WebConfiguration.java:48) start initializing pager info
241:2014-03-05 21:30:08 [INFO ](SocialService.java:44) MUTUAL_MATE_QUEUE_SIZE                           : 500
242:2014-03-05 21:30:08 [INFO ](SocialService.java:45) MUTUAL_MATE_THREADS                                      : 10
243:2014-03-05 21:30:08 [INFO ](SocialService.java:46) MUTUAL_MATE_QUEUE_MONTIR_THRESHOLD       : 450
244:2014-03-05 21:30:08 [INFO ](SocialService.java:52) MutualMateThread : Thread Run 0
245:2014-03-05 21:30:08 [INFO ](SocialService.java:52) MutualMateThread : Thread Run 1
246:2014-03-05 21:30:08 [INFO ](SocialService.java:52) MutualMateThread : Thread Run 2
247:2014-03-05 21:30:08 [INFO ](SocialService.java:52) MutualMateThread : Thread Run 3
248:2014-03-05 21:30:08 [INFO ](SocialService.java:52) MutualMateThread : Thread Run 4
{code}

Q: 파일 또는 스트림을 정렬하면서 중복은 제거하고 싶다.
A: sort \-u 명령을 이용한다.
{code}
## WARN이 있는 부분을 찾아서 중복을 제거하고 싶다.
[localhost /home/user]# grep -n "WARN" bloc_10850.out.2014.03.05 | sort -u
{code}

Q: 키워드를 찾아서 중복을 제거한 텍스트만 보고 싶은데 날짜와 시간이 있어서 중복 제거한 값만 보기가 너무 힘들다.
A: awk 명령을 이용해서 날짜와 시간을 제거하고 나머지 정보만 출력하면 된다.

{code}
[localhost /home/user]# grep -n "WARN" bloc_10850.out.2014.03.05  | awk '{printf("%s\t%s\t%s\n", $3, $4, $5)}' | sort -u
[WARN   ](CacheCron.java:41)    Cache
[WARN   ](CacheCron.java:43)    Cache
[WARN   ](ClientCnxn.java:1185) Session
{code}

Q: 파일 또는 스트림을 정렬하면서 중복은 제거한 갯수를 알고 싶다.
A: sort와 uniq 명령을 조합해서 결과를 뽑는다.

{code}
[localhost:/home1/user] cat displayAllText.txt | sort | uniq -c
{code}
{code}
[localhost /home/user]# grep -n "WARN" bloc_10850.out.2014.03.05  | awk '{printf("%s\t%s\t%s\n", $3, $4, $5)}' | sort | uniq -c
    149 [WARN   ](CacheCron.java:41)    Cache
    149 [WARN   ](CacheCron.java:43)    Cache
     18 [WARN   ](ClientCnxn.java:1185) Session
{code}

Q: 단순하게 파일 또는 스트림의 레코드 갯수를 알고 싶다.
A: wc 명령을 이용한다.

{code}
[localhost:/home1/user] cat displayAllText.txt | wc -l
{code}

Q: 최대 열수 있는 파일의 갯수를 알고 싶다.
A: ulimit  명령을 이용하면 됩니다.

{code}
## ulimit - get and set user limits
[localhost:/home1/user] ulimit -a
## 출력 화면에서 open files에 해당하는 값을 보면됨.
{code}

Q: ulimit 의 설정을 저장하고 싶다면
A: /etc/security/limits.conf 파일을 고치면 됩니다.

Q: vi 명령어가 궁금합니다.

A:&nbsp; !vi-vim-cheat-sheet-ko.png|border=1!

Q: Command mode로 가고 싶을 때는?
A: Esc 키를 누릅니다.

Q: vi에서 파일의 맨 끝으로 가고 싶을 때는?
A: Command mode에서 Shift + G 키를 누릅니다.

Q: vi에서 파일의 맨 처음으로 가고 싶을 때는?
A: Command mode에서 gg 키를 누릅니다.

Q: vi에서 아래줄의 내용을 위로 올리고 싶을 때는?
A: Command mode에서 Shift + J 키를 누릅니다.

{code}
1: rm -f install.sh
2: touch install.sh
#########################
1: rm -f install.shtouch install.sh
{code}

Q: vi 창을 하나 더 열고 다른 파일을 열고 싶을 때는?
A: 아래의 순서로 명령을 입력한다.
* Ctrl + V , W 키를 눌러서 vi 창을 2등분한다. 
* command mode에서 :e filename으로 파일을 연다

!vi_dual_screen.jpg|border=1!

Q: 좌측편에 디렉토리를 보면서 파일을 편집하고 싶다.
A: 아래의 순서로 명령을 입력한다.
* Ctrl + V , W 키를 눌러서 vi 창을 2등분한다. 
* command mode에서 :e .으로 파일을 연다
* 이동 키(j, k키)로 열고 싶은 파일로 이동한 후에 Shift + P 키를 누른다.
** 참고 사항 : Shiftp + P 키 대신에 Enter 키를 누르면 반대편 창이 아닌 현재 창에 파일이 열린다.
* 반대편 창에 해당 파일이 열린다.


Q: 열려져 있는 두개의 창사이를 이동하고 싶다.
A: Ctrl + w, w 키를 누르면 창 사이를 이동할 수 있다.

Q: 두 개(A, B)의 파일을 열어서 한쪽 파일(A)의 내용을 다른 파일(B)로 복사하고 싶다.
A: 아래의 순서로 명령을 입력한다.
* vi A를 입력한다.
* Ctrl + V , W 키를 눌러서 vi 창을 2등분한다. 
* command mode에서 :e B으로 파일을 연다
* 복사하려고 하는 행으로 이동한다.
* y키를 2번 누른다.
* Ctrl + w, w키로 반대편 창으로 이동한다.
* Command mode에서 p 키를 누른다.
** 현재 커서가 위치하는 행의 아래 행에 데이터가 붙는다.
** 잘못 붙여넣기를 했으면 command mode에서 u키를 누르며 붙여넣기한 것이 사라진다.