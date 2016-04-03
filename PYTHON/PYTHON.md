아래와 같이 코딩한 다음 python을 실행시키면 다음과 같은 오류가 발생한다.

```python
#!/usr/bin/python

print "한글입력하면 실패한다."
```

```
sys:1: DeprecationWarning: Non-ASCII character '\xed' in file ./testCodingCharacter.py on line 3, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details
한글입력하면 실패한다.
```

이유는 python은 기본적으로 소스가 ascii로 작성되었음을 가정하고 컴파일을 수행한다. 그런데 한글이 들어가 있으니 인식을 못하는 거다.

이를 해결하려면 다음과 같이 설정해주면된다.

```python
#!/usr/bin/python
# coding=cp949

print "한글입력하면 실패한다."
```
또는

```python
#!/usr/bin/python
# vim: set fileencoding=cp949

print "한글입력하면 실패한다."

```

또는 

```python
#!/usr/bin/python
# -*- coding: cp949 -*-

print "한글입력하면 실패한다."
```

아래 url을 참조하면 더 자세한 정보를 얻을 수 있다.
http://www.python.org/dev/peps/pep-0263/

그런데 아래와 같이 작업하니까 segmentation fault가 발생하더라

```python
#!/usr/bin/env python
# coding=cp949

print "한글입력하면 실패한다."


#!/usr/bin/env python
# vim: set fileencoding=cp949

print "한글입력하면 실패한다."


#!/usr/bin/env python
# -*- coding: cp949 -*-

print "한글입력하면 실패한다."
```

단지 #!/usr/bin/python을 #!/usr/bin/env python으로만 바꾸었을 뿐인데..
하지만 또 아래와 같이 설정하면 잘 된다.
#!/usr/bin/env python 과 cp949가 어떤 연관이 있는 지는 잘 모르겠지만..
어쨌든 내 시스템에서는 에러가 났다...ㅎㅎ

```python
#!/usr/bin/env python
# -*- coding: utf8 -*-

print "한글입력하면 실패한다."


#!/usr/bin/env python
# coding=utf8

print "한글입력하면 실패한다."


#!/usr/bin/env python
# vim: set fileencoding=utf8

print "한글입력하면 실패한다."
 
```

그래서 난 이렇게 쓰기로 했다.


```python
#!/usr/bin/env python
# coding=utf8

print "한글입력하면 실패한다."
s```