---
title: '[React] styled-components를 이해해보자'
tags: ["react"]
published: true
date: '2021-07-07'
---

Styled-Components를 이해한대로 정리해보기
===

이번에 css in js를 한 번 공부해보자는 생각을 가지고, 가장 많이 쓰이는 라이브러리인 styled-components를 공부해보았다. 그렇게 내가 이해한 바를 최대한 글로 남겨보려고 한다. 조금은 두서가 없어도 이해 바란다.

---

## styled-components의 결과물


styled-components의 코딩방법은 다음과 같다.
```javascript
import styled from 'styled-components'

const Example = styled.div`
    background-color : black;
    width : 100%;
`;
```
문법적인 원리는 [벨로퍼트님의 설명](https://react.vlpt.us/styling/03-styled-components.html)에 자세하게 나와있다. 
중요한 점은 **styled-components는 component를 결과물로 생성**해낸다는 것이다. 따라서 style을 지정해놓는 문서를 따로 빼놓아서 컴포넌트는 컴포넌트 별로. style은 style별로 구분해서 코딩을 할 수 있다는 점이다. 

---

## styled-components의 전역적인 관리
기존의 css에서 :root 를 통해서 전역 변수를 관리하던 것을 기억할 것이다. styled-components에서도 자신의 자식 컴포넌트에 모두 적용이 가능하게 하는 전역적인 관리 방법이 존재한다. 내가 이해하기로는 두 가지 방법이었는데, **createGlobalStyle** 과 **themeProvider**였다.

### 1. themeProvider
themeProvider는 react의 context API를 활용하여 전역 변수 관리 기능을 제공한다. 사용방법은 다음과 같다.

```javascript
// src/style/theme.js 에서 theme 정의 후 export

// App에서
import { ThemeProvider } from 'styled-components';
import { theme } from '../src/theme';

const App = () => {
    ...

    return (
        <ThemeProvider theme={theme}>

        <ThemeProvider/>
    )

}
```
대략적인 코딩은 위와 같다. context API 처럼 Provider로 감싸버리고 theme props로 theme을 내려주는 것이다. 이렇게 하면, 자식 컴포넌트들은 모두 theme을 props로 활용할 수 있게 된다. 

이 때, 자식의 컴포넌트도 위에서 말했던 것 처럼 css부분과 컴포넌트 부분을 분리한다면 다음과 같다.

```javascript
// /style/Container-style.js에서 

import styled, { css } from 'styled-components';

// 이 처럼 컴포넌트를 결과물로 내뱉기 때문에 {theme}을 props로 사용하는 것이다. 
const ChangeButton = styled.button`
    ${({theme})=>{
        return css`
            background-color : ${theme.colors};
        `;
    }}
`;

// 여러개의 component들을 객체에 넣어주고 export default할 수 있는 장점이 있다.
const styledComponents = { ChangeButton };
export default styledComponents;

// component 파일에서는 다음과 같은 작업이 이루어진다.
import styledComponents from '../src/style/Container-style.js';
const { ChangeButton } = styledComponents;

const Container = () => {
    return (
        <ChangeButton />
    )
}

export default Container;

```
이 처럼 styled-componets를 이용해 style이 폴더의 파일에서 component를 정의한 후, 이를 컴포넌트들이 담겨 있는 파일에서 불러과 레고 쌓듯이 활용하면 된다.

### 2. createGlobalStyle

styled-components의 themeProvider가 변수를 건내주었다면, createGlobalStyle은 global한 style 내용을 담고 있다. 

```javascript
//src/style/GlobalStyle.js에서
// GlobalStyle도 기본적인 사용법은 같다.

import { createGlobalStyle, css } from 'styled-components';

const GlobalStyle = createGlobalStyle`
    ${({theme})=>{
        return css`
            body {
                font-size : ${theme.fonts.base}
            }
        `;
    }}
    
    html {
        font-size : 70%;
    }
`;

export default GlobalStyle;

//중요한 점은 App에서이다.

import GlobalStyle from './src/style/GlobalStyle';

...

return (
    <ThemeProvider theme={theme}>
        <GlobalStyle />
    </ThemeProvider>
)
// ThemeProvider가 제공해주는 theme을 사용하기 위해 그 자식 컴포넌트로 들어와야 한다.

```

---

이상으로 styled-components의 간단한 사용법을 알아보았다. 여기에 styled-reset, styled-normalize등 다양한 라이브러리와 scss, sass의 문법 등 더 다루어야할 내용이 많지만... 다음 기회에 더 적어보도록 하겠다.

### Reference
- [코드 도움 받은 블로그](https://dkje.github.io/2020/10/13/StyledComponents/)
- [ThemeProvier 내용](https://wonit.tistory.com/366)
- [styled-component에 대한 전반적인 내용](https://insindema.tistory.com/49)

