---
title: '[React] react component hierarchy를 design하는 법'
tags: ["react"]
published: true
date: '2021-07-08'
---

리액트 컴포넌트 구조를 정석대로 디자인해보자
===
이번에 react를 사용해서 to-do-list를 구현하려다 막상 트라이해보려고 하니 어디서부터 시작할지 막막했다. 그래서 컴포넌트 구조를 어떻게 짤 지를 설명해주는 기초적인 기사를 하나 정리해보려고 한다. 어찌보면 당연한 이야기이지만 정리해서 기억해두려는 것을 목표로 글을 써본다. 

기사에서 소개하는 react component design 방법은 크게 두 가지 단계로 나뉜다.

1. Break a layout into react components
2. Convert those components into a component hierarchy

---

## 자세한 순서

### STEP 1. Wireframing

내가 만들고 싶은 사이트의 구조를 간략하게 네모 박스로 표기하여 그림을 그린다. 

![와이어프레임](https://miro.medium.com/max/945/1*IospKIefKTIVIh9pLYrjug.png)

### STEP 2. 컴포넌트로 쪼개 보기

컴포넌트로 쪼개려면 컴포넌트의 의미를 알아야 한다.

> A component is anything that has its standalone meaning in the layout.

즉, **기능을 하는 가장 작은 단위**인 것이다. 이에 입각하여 잘게 쪼갠다. 위의 사진을 쪼개 보면

- App : 다른 컴포넌트를 아우르는 컴포넌트 (Main Container)
- Intro : 안에 있는 글과 button 컴포넌트를 랜더링한다. (Introduction page)
- Button : (button)
- Skills : (Skills page)
- SkillItem : (items in skills page)

### STEP 3. 컴포넌트 구조를 그려보기

이렇게 쪼갠 컴포넌트를 서로 간의 관계를 그려본다. 방식은 tree구조로 그리면 된다. 

![](https://miro.medium.com/max/945/1*MXsfkoC2XdKCLBdtn6EX2Q.png)

이런 식으로 그려놓고 내려주는 props까지 적어주면 끝난다!

---

### refernce 
[The Best Approach To Design React Component Hierarchy](https://medium.com/javascript-essentials/the-best-approach-to-design-react-component-hierarchy-978bb152dbb2)


