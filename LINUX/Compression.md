#### Q: 디렉토리의 모든 파일을 하나의 파일로 만들고 싶다.
A: tar 명령을 이용한다.

```
tar cf archieve_file.tar ./source
```

#### Q: 파일의 크기를 줄여서 압축하고 싶다.
A: gzip 명령을 이용한다.

```
gzip archieve_file.tar
```

#### Q: gz 파일을 풀고 싶다.
A: gunzip 명령을 이용한다.

```
gunzip archieve_file.tar.gz
```

#### Q: 다수의 파일을 하나의 파일로 만들면서 압축도 하고 싶다.
A: tar 명령을 이용한다.

```
tar -czf archieve_file.tar.gz ./source
tar -czvf archieve_file.tar.gz ./source
```

#### Q: tar.gz으로 압축된 파일을 압축도 풀면서 다수의 파일로 만들고 싶다.
A: tar 명령을 이용한다.

```
tar -xzf archieve_file.tar.gz
tar -xzvf archieve_file.tar.gz
```


#### Q: tar.gz 포맷 말고 윈도우에서 많이 쓰는 zip 파일로 압축하고 싶다.
A: zip 명령을 이용한다.

#### Q: zip 파일의 압축을 풀 때에는?
A: 당연히 unzip 명령을 이용한다.