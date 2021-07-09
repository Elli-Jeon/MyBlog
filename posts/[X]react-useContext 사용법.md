---
tags:
  - react
published: false
date: 2021-07-10
title: '[React] useContext 사용법'
---

useContext 사용법

STEP 1. createContext, useContext를 가져온다.

```javascript
import React, { useContext, createContext } from 'react'
```

STEP 2. 새로운 변수에 createContext를 담고 인자로 기본값을 준다.

```javascript
const NewContext = createContext(null);
```

STEP 3. 최종 전달하여 사용하고픈 컴포넌트에서 useContext로 newContext를 가져온다.

```javascript
const Child = () => {
    const text = useContext(newContext);
    return (
        <div>
            {text}
        </div>
    );
}
```

STEP 4. context.Provider 컴포넌트로 랜더링한 컴포넌트를 감싸준다. value에 전달하고 싶은 값을 설정한다. 

```javascript
const App = () => {
    return (
        <NewContext.Provider value='elli'>
            <Child/>
        <NewContext.Provider/>
    );
}


```

reference

[원문](https://xiubindev.tistory.com/105)
