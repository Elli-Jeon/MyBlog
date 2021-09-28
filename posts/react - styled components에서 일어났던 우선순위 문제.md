---
title: "[react] styled-components에서의 우선순위 문제"
tags: ["javascript", "react"]
published: true
date: "2021-09-28"
---

<h1 align="center"> styled-components에서의 우선순위 문제</h1>

## 사건 개요
오늘도 즐겁게 스크림도르 코딩을 하던 중에 동료의 작업을 수정하다가 이상한 일이 발생했다. 코드로 함께 보자.


기본적으로 나는 page에다가 BaseTemplate을 두어서 여기에서 header 밑의 main container의 구조를 잡고 작업한다. 

```javascript
// BaseTemplate/index.jsx

import React from 'react';
import MainContainer from './styles';

const BaseTemplate = ({ children }) => {
  return <MainContainer>{children}</MainContainer>;
};

export default BaseTemplate;

// BaseTemplate/styeld.js

const MainContainer = styled.div`
  flex: 1;
  min-height: 0;
  width: 100%;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 7;
  // Navigation
  .Navigation {
    height: 100%;
    border: none;
  }
  ...

`

```


```javascript
// matching/index.jsx

 return (
    <BaseTemplate>
      <Navigation />
      <MatchingContainer aside={aside}>

      {...}

      </MatchingContainer>
    </BaseTemplate>
 )

// matching/style.js

export const MatchingContainer = styled.section`
  min-height: 0;
  height: 100%;
  width: 950px;
  position: relative;
  display: flex;
  align-items: start;
`

```

해석해보자면 이러하다, Base template에서 `section`이라고 스타일링을 하고서 그 아래 컴포넌트에서 (matching의 matchingcontainer) section을 다시 스타일링 해주었으니 *base templte의 스타일링 위에 덧씌워질 것이라고 생각을 하였다.*

문제는 그렇지 않았다는 것이다. 내가 생각한 것과는 반대로 스타일링은 matchingcontainer의 스타일링 위에 base template의 스타일링이 씌워진 것이다. 

당시에는 그냥 넘기고서 작업을 했는데, 집에 돌아와서 생각해보니 내 상식과는 반대로 작동이 되어서... 

일단 이러한 블로그를 찾았다. 

[extending styles가 잘 안될때](https://velog.io/@haebin/styled-component-Extending-Styles%EC%9D%B4-%EC%95%88%EB%90%A0-%EB%95%8C)

블로그의 내용은 "다른 component의 style을 수정한 새로운 component를 사용하고 싶다면 기존의 component에서 props.className를 전달받도록 작성하자!" 라는 것이었다. 


```javascript```


```javascript```