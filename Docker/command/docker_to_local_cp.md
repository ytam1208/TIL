cp의 명령어는 쉽다.
docker cp 원천파일경로 복사할곳경로를 적으면 된다.
 

호스트 정보는 경로 그대로 적으면 된다.

ex1) /ubuntu/

ex2) /ubuntu/test.txt

 
container 정보는 container_name:경로와 같이 컨테이너 이름과 경로 사이에 : 이걸 적어서 적어주면 된다.

ex1) test_container:/root/data/

ex2) test_container:/root/data/test.txt
 

아래 예시를 보면 정확하게 알 수 있다.

```
docekr cp 컨테이너이름:컨테이너내부_파일경로 호스트_경로
```

![1](https://user-images.githubusercontent.com/56625848/189635630-828c51a0-803f-428a-8d15-6d5faef2520d.png)

만약에 도커 안에 있는 test.txt 파일을 내 local로 옮기고 싶다면, 

![2](https://user-images.githubusercontent.com/56625848/189635739-be763b50-9197-423c-91af-796fbe6b432d.png)

위와 같이 하면 파일을 옮겨 올 수 있다.