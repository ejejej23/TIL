## 커밋 내역을 다시 수정해 올리고 싶을 때
: 깃 히스토리에서 다시 올리고 싶은 커밋을 체크하고 그부분에서 interactively rebase from here 실행
-> 깃에 포스푸시

-----

1. git config user.email을 통해 연결된 깃헙 계정이 깃헙 계정과 정보가 다르면 잔디가 심기지 않는다. 
아래와 같이 실행해주자

```
git config --global user.name 내이름
git config --global user.email 내계정
```

2. fork 한 레포지토리에는 커밋해도 잔디 안심어짐
- new 레포지토리 하고 거기다 올리면 심어짐

3. branch 커밋시 잔디 안심어짐
- master 브랜치에 merge시키는 경우 커밋들이 잔디추가됨