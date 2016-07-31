#### Q : 특정 프로그램이 실행이 안되는 동적으로 참조하는 라이브러리 정보를 알고 싶습니다.
A : ldd 명령을 사용하시면 됩니다.

```
[localhost:/home1/user] ldd /bin/traceroute
        linux-gate.so.1 =>  (0x0037c000)
        libm.so.6 => /lib/i686/nosegneg/libm.so.6 (0x00a59000)
        libc.so.6 => /lib/i686/nosegneg/libc.so.6 (0x008d8000)
        /lib/ld-linux.so.2 (0x008b9000)

# v 옵션을 추가하면 더 세부적인 정보를 보실 수 있습니다.
[localhost:/home1/user] ldd -v /bin/traceroute
        linux-gate.so.1 =>  (0x00836000)
        libm.so.6 => /lib/i686/nosegneg/libm.so.6 (0x00a59000)
        libc.so.6 => /lib/i686/nosegneg/libc.so.6 (0x008d8000)
        /lib/ld-linux.so.2 (0x008b9000)

        Version information:
        /bin/traceroute:
                libc.so.6 (GLIBC_2.4) => /lib/i686/nosegneg/libc.so.6
                libc.so.6 (GLIBC_2.3) => /lib/i686/nosegneg/libc.so.6
                libc.so.6 (GLIBC_2.1) => /lib/i686/nosegneg/libc.so.6
                libc.so.6 (GLIBC_2.3.4) => /lib/i686/nosegneg/libc.so.6
                libc.so.6 (GLIBC_2.0) => /lib/i686/nosegneg/libc.so.6
        /lib/i686/nosegneg/libm.so.6:
                ld-linux.so.2 (GLIBC_PRIVATE) => /lib/ld-linux.so.2
                libc.so.6 (GLIBC_2.1.3) => /lib/i686/nosegneg/libc.so.6
                libc.so.6 (GLIBC_2.0) => /lib/i686/nosegneg/libc.so.6
        /lib/i686/nosegneg/libc.so.6:
                ld-linux.so.2 (GLIBC_PRIVATE) => /lib/ld-linux.so.2
                ld-linux.so.2 (GLIBC_2.3) => /lib/ld-linux.so.2
                ld-linux.so.2 (GLIBC_2.1) => /lib/ld-linux.so.2
```

