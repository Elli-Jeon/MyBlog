---
title: '[Git] Git 명령어(command) 정리해보자 (1)'
tags: ["git"]
published: true
date: '2021-07-06'
---

Git 명령어 정리 (1)
===

그동안 Github를 사용하면서 Github Desktop을 주로 사용해서 개발을 진행했었는데, 이번에 프로젝트를 진행하면서 이 기회에 Git을 CLI를 통해 써보자고 마음을 먹었다. 그래서 한 번 지금 배운 Git Command들을 한 번 정리해보는 시간을 가지려고 한다.  

## Github에서 clone해 올 때.

가장 먼저 Github에서 리포지토리를 생성하고 이 것을 로컬 리포지토리에 가져오고 싶은 상황이다. 

Github와 연동시키려는 폴더에 들어간 후.

```
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin [url]
git push origin master
```
위와 같이 입력해주면 로컬 리포지토리 생성 완료!

## Github에 push 하는 방법

작업이 끝나면 지금까지의 변경 사항을 저장하고 싶을 것이다. 지금까지의 파일 변경 사항을 git이 추적해주고 있기 때문에 어떤 파일이 변경되었는지 우리에게 알려준다. 

```
git status
git add .
git commit -m "bla-bla-"
git push origin [branch명]
```

## Branch 사용법

main branch에서 바로 작업을 진행하다보면 여러번의 commit으로 더러워(?)지는 것을 목격할 수 있다. 그렇기에 주로 branch를 새로 내려서 작업을 진행한다.

현재 branch 목록을 확인하려면
```
git branch
```

해당 branch로 이동하려면
```
git checkout [branch명]
```

새로운 branch 생성과 삭제는
```
git branch [branch 명]
git branch -d [branch명]
```
이다. 

만약 branch를 생성하고 동시에 checkout(이동)까지 하고 싶다면, 
```
git checkout -b [branch명]
```
이라고 적어주면 된다.


- - -
 
쓰다보니 지금 내가 사용하는 기초적인 command 사용 방법만 적게 된 것 같다... 처음 md로 글을 써보니 어색하기도 해서 적응 시간이 좀 필요할듯 하다.