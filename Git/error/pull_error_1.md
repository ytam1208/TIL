```
error: Your local changes to the following files would be overwritten by merge: 
...
Please commit your changes or stash them before you merge.
```

원격저장소 GIT에서 로컬로 파일을 Pull 하던 주에 에러 메세지 발생 시

문제의 원인은 원격 저장소에서 Pull 할 때 로컬에 수정한 파일이 존재하기 때문이다.


1) 로컬 내 수정사항을 임시 공간으로 옮긴다.

```
git stash
```

2) 원격 저장소를 pull한다.

```
git pull origin branch_name
```

3) 로컬 내 수정사항과 원격저장소에서 pull 한 파일을 병합(merge)한다.

```
git stash pop
```

