#### Q : 내가 누군 지 알고 싶어요.
A : whoami로 확인하세요..

#### Q : 다른 계정으로 바꾸고 싶어요.
A : su 를 이용하세요.

#### Q : 다른 계정으로 잠깐 명령을 실행하고 싶어요.
A : sudo를 사용하세요.

#### Q : sudo를 이용했는데 비밀번호를 물어보지 않게 할 수는 없나요?
A : root의 권한이 필요할 경우에는 /etc/sudoers 를 수정하시면 됩니다.
   그외의 유저의 경우에는  /etc/sudoers.d/mysudoers 로 생성하시면 됩니다.
   파일의 권한은 반드시 440이어야 합니다.

#### Q : 파일을 수정하려니 안되요..
A : 해당 유저에게 쓰기 권한이 있는 지 확인하세요.

* 파일의 권한
ls -l 명령으로 파일의 권한을 확인할 수 있습니다.
```
[localhost /home1/user ]# ls -l
drwxrwxr-x  3 user  group  4096  8월 19 18:38 bin
-rwxrw-r--  1 user  group  6689  9월 10  2012 command.sh
```
    * [file type][user permission][group permission][other permission]
    * d는 디렉토리, r은 read, w는 write, x는 execute 입니다. 

* 파일 타입 

| file type | 설명 |
|--|--|
| \- | 일반 파일 |
| b | 블럭 특수 파일 |
| c | 문자 특수 파일 |
| d | 디렉토리 |
| l | 심볼링 링크| 

* chown : 소유주를 바꾸는 명령입니다. 
```
chown user:group file로 하면 group 정보도 한꺼번에 바꿀 수 있어요.
```
* chgrp : 파일의 그룹을 바꾸는 명령입니다.
* chmod : 파일의 권한을 수정하는 명령입니다.
    * 일반적으로 숫자로 권한을 수정합니다.
        * read : 4, write : 2, execute : 1
        * 읽기 권한만 주고 싶다. : 4 (read) 
        * 읽기와 쓰기 권한만 주고 싶다. : 6 (read + write) 
        * 읽기, 쓰기, 실행 권한 모두 주고 싶다. : 7 (read + write + execute)

```
 위의 명령 3개를 이용하면서 하위 디렉토리도 모두 바꾸고 싶다면 -r 옵션이 아니라 -R 옵션을 주어야 합니다. 
```

#### Q : 파일을 읽고, 쓰고, 실행은 가능하지만 삭제는 못하게 하고 싶어요.. 쓰기 권한이 있는 데 파일 삭제가 안되요.
A : sticky bit를 설정해주시면 됩니다. sticky bit가 설정되어 있는 지 확인하세요.

```
[localhost /home1/user ]# ls -l
-rwxrwxrwx  1 www www 6689  9월 10  2012 command.sh
[localhost /home1/user ]# chmod 1777 command.sh
[localhost /home1/user ]# ls -l
-rwxrwxrwT  1 www www 6689  9월 10  2012 command.sh

```
* sticky bit가 설정되어 있는 경우에 삭제가 가능한 유저
    * 파일의 소유주
    * 상위 디렉토리의 소유주
    * root

#### Q : ls -l을 했던 rwx 외의 다른 문자가 있어요..
A : setuid, setgid 입니다.

* 해당 명령을 실행할 때 소유주나 그룹의 권한을 주고 싶은 경우에 설정합니다.
* 예제
    * 비밀번호를 변경하려면 /etc/shadow 파일을 수정하고 싶지만 권한이 부족함
    * passwd 명령을 이용해서 비밀번호를 변경함
    * passwd 명령은 uid가 셋팅되어 있으므로 root의 권한으로 실행이 가능함.

```
[localhost /home1/user ]# ls -al /etc/shadow
-r-------- 1 root root 651 10월  1 11:03 /etc/shadow
[localhost /home1/user ]# ls -al /usr/bin/passwd
-rwsr-xr-x 1 root root 27936  8월 11  2010 /usr/bin/passwd
```

| SUG | | User | number  |  Group | number | Others  | number |
|-----|-|------|---------|--------|--------|---------|--------| 
| Sticky bit | 1000 | R | 400  |  R | 40 | R | 4 |
| GID | 2000 | W | 200  | W | 20 | W | 2 |
| UID  | 4000 | X | 100 | X | 10 | X | 1 |
 

#### Q : 윈도우의 바로가기 처럼 파일은 하나이고 링크를 걸고 싶습니다.
A : 심볼릭 링크를 걸어주세요..

```
# bin 디렉토리 및에 링크를 걸고 싶은 경우
* ln -s [원본] [링크] 임.  혼동하기 쉬움.
[localhost /home1/user ]# ln -s /home1/user/command.sh /home1/user/bin/command.sh
```

