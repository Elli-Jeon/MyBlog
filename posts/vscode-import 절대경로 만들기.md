---
title: '[VsCode] import 절대 경로 만들기'
tags: ["vscode"]
published: true
date: '2021-07-23'
---

## VS code에서 import 절대 경로 만드는 법

react에서 워낙 import를 많이 쓰다보니 일일히 상대경로로 가져오는 것이 힘들어 방법을 찾아보았다.

root 디렉토리에 **jsconfig.json**을 만들어준다. (vscode 공식문서에 따르면 이 파일이 있는 위치가 root 디렉토리가 된다고 한다.)

```json
// jsconfig.json

{
    "compilerOptions" : {
        "baseUrl" : "src"
    }, 
    "include" : [
        "src"
    ]
}
```

이러면 이제 src를 기준으로 파일을 import할 수 있다. 
```javascript
import *** from 'components/Home'
```
이런 식으로.

jsconfing.json 자체가 typeScript의 설정 파일인 tsconfig.json에서 파생된거라고 하니.... 아마 앞으로도 몇 번 볼일이 생길 것 같다.

<a href="https://code.visualstudio.com/docs/languages/jsconfig" tarbet="_blank">vscode 공식 문서</a>