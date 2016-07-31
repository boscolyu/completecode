#### Q: 특정 파일이 탭 또는 공백으로 구분되어 있는데, 2번째 부분만 출력하고 싶다.
A: awk 명령을 이용한다.

```
awk '{print $2}' file
cat file | awk '{print $2}'
```


#### Q: 특정 파일이 쉼표로 구분되어 있는 데 3번째 컬럼만 출력하고 싶다.
A: awk 명령을 이용한다.

```
awk -F"," '{print $2}' file
```

#### Q: 파일의 특정 컬럼에 특정값이 있는 갯수를 알고 싶다.
A: awk 명령을 이용한다.

```
awk 'BEGIN{sum = 0}{if ($2 == "TEXT") {sum = sum + 1} }} END{print sum}}'
```

#### Q: 파일의 특정 컬럼의 값에 앞뒤로 내가 문자열을 넣어주고 싶다.
A: awk 명령을 이용한다.

```
awk '{printf("FORMAT = %s\t%s\t%s\n", $1, $3, $5)}'
```