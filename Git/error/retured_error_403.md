<img width="481" alt="git_error(403)" src="https://user-images.githubusercontent.com/56625848/187660630-1d6111ef-a7af-4c6b-8158-822c9c35ec0f.png">

git push 중에 생긴 The requested URL returend error: 403

해결방안

```
git remote set-url origin "https://깃-유저-네임@github.com/깃-유저-네임/github-repository-name.git"

(이때, 큰따옴표는 제거)

git push -u origin main 

이후에 다시 새롭게 깃 아이디와 토큰 패스워드를 입력하라고 나옴
```