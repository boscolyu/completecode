#### Q : Shell이란? Shell script란?

A : 커널과 사용자를 연결해주는 인터페이스.

#### Q : 그럼 시스템에서 지원하고 있는 Shell이 어떤 것이 있는 지 확인하는 방법은?

A : 
```
[localhost:/home1/user] cat /etc/shells
```

#### Q : Shell의 종류는?

A : sh, bash, csh, tcsh 가 있다.

#### Q :  많이 쓰는 Shell은?

A : bash를 많이 쓴다. 이후 아래 설명은 모두 bash 기준으로 설명

#### Q :  내가 사용하는 Shell이 무엇인지 확인하는 방법

A :
```
[localhost:/home1/user] echo $SHELL
```

#### Q :  나만의 Shell의 환경을 구축하고 싶다면

A : 홈디렉토리에서 .bashrc를 수정한다.


#### Q :  .bashrc, .bash_profile, /etc/profile, /etc/bash.bashrc의 차이는?

A : /etc/profile은 전체 유저의 로그인 설정, /etc/bash.bashrc는 bash 사용자의 전체 설정임.
    ~/.bash_profile, ~/.bashrc 는 각 유저별 설정을 하는 파일임.


#### Q :  홈디렉토리로 가는 방법은?

A : 
```
[localhost:/home1/user] cd
```

#### Q :  수정한 .bashrc를 적용하고 싶다면?

A : 로그아웃한 다음 로그인을 다시한다.

#### Q :  수정한 .bashrc를 적용하고 싶지만 로그아웃은 하고 싶지 않다면?

A : 
```
[localhost:/home1/user] source ./.basrhc
```

#### Q :  환경 변수는?

A : 윈도우의 환경 변수와 비슷한 개념

#### Q :  Command not found.라는 에러가 발생해요..
```
[localhost:/home1/user] ifconfig
ifconfig: Command not found.
```

A : PATH에 해당 명령어가 포함되어 있지 않아서 그래요.

#### Q :  현재 PATH는 어떻게 확인하죠?

A : echo $PATH라고 입력하면 됩니다.
```
[localhost:/home1/user] echo $PATH
/home1/user/bin:/usr/local/bin:/usr/local/sbin:/bin:/usr/bin:/usr/sbin:/usr/ucb:/etc:.
```

#### Q :  명령어가 PATH에 포함되어 있는 지는 어떻게 확인하죠?

A : which 명령어를 통해서 확인할 수 있어요.
```
   [localhost:/home1/user] which ifconfig
```

#### Q :  PATH에 포함시키고 싶은데 명령어가 어디있는 지 모르겠어요..

A : whereis 명령어를 이용해서 확인할 수 있어요.
```
   [localhost:/home1/user] whereis ifconfig
```

#### Q :  예전에 입력했던 명령어를 보고 싶어요

A : history 명령을 이용하세요.
```
   [localhost:/home1/user] history
```

#### Q :  bash shell script를 개발하기 위해서 유용한 페이지는?

A : 여기로 가보세요.. --> http://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/


#### Q :  secureCRT는 못쓰고 putty를 쓰고 있는데 putty창을 여러개 띄우고 쓰려니 너무 불편해요

A : screen을 사용하세요..

#### Q :  screen도 환경설정 파일이 있나요?

A : 네 ~/.screenrc 파일을 만들어서 아래처럼 넣어주시면 되요..

```
screen -t hite95 0

bind c screen -t hite95
bind ^C screen -t hite95

bindkey "\033[17~" select 0
bindkey "\033[18~" select 1
bindkey "\033[19~" select 2
bindkey "\033[20~" select 3
bindkey "\033[21~" select 4
bindkey "\033[23~" select 5
bindkey "\033[24~" select 6

defscrollback 10000

vbell off
caption always "%{= bc}%?%-Lw%?%{= bY}%n*%f %t%?(%u)%?%{= bc}%?%+Lw%?"
hardstatus      off

```

#### Q :  screen의 좋은 점은요?

A : shell을 그대로 살려둘 수 있어요..
    출력된 화면을 vi 단축키를 이용해서 이동할 수 있어요..
    화면간 창이동이 편리합니다.

#### Q :  콘솔로 출력되는 걸 없애버리고 싶어요.

A : 아래와 같이 redirect 시켜버리세요..
```
   [localhost:/home1/user] ./command.sh > /dev/null 2> /dev/null
```

#### Q :  프로그램을 백그라운드 프로세스(데몬)으로 띄우고 싶어요..

A : 아래와 같이 실행 시키세요.
```
   [localhost:/home1/user] ./command.sh > /dev/null 2> /dev/null &
   [localhost:/home1/user] nohup ./command.sh > /dev/null 2> /dev/null & (권장)
```

#### Q :  nohup에 대한 자세한 내용이 알고 싶어요?

A : 아래 링크가 괜찮은 것 같네요
    http://en.wikipedia.org/wiki/Nohup