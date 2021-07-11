---
title: '[React] useReducer Hook 이해하기'
tags: ["react"]
published: true
date: '2021-07-12'
---

useReducer Hook에 대한 이해
===

어떠한 것에 대해 이해하기 위해 가장 중요한 것은 **탄생한 목적과 목표**에 대해 이해하는 것이다. 그러한 사실에 입각하여 react의 useReducer Hook에 대해 이해해보고자 한다.

### useReducer의 목표

useReducer는 무엇을 이루고자 할까?

> useState와 비교했을 때, useReducer는 컴포넌트 안에서 이루어지던 상태 관리를 component 바깥으로 뽑아서 사용 가능하게 해준다. 

그림으로 보면 이해가 빠르다.

![](../src\images\usereducer.png)

1. 컴포넌트 바깥에 reducer함수를 두고,
2. 컴포넌트 안 쪽에서 useReducer를 배정하고
3. 컴포넌트 안 쪽의 dispatch를 통해서 
4. 컴포넌트 바깥의 reducer의 action을 통해 상태가 변화한다. 

위의 과정이 핵심 과정이다. **reducer로 컴포넌트의 상태 관리를 뽑아쓴다고** 이해하면 편하다. 자세한 내용은 아래의 링크를 참조하자. 

---

### reference

<a href="https://react.vlpt.us/basic/20-useReducer.html" target="_blank">velopart 님의 설명</a>

<a href="https://www.youtube.com/watch?v=kK_Wqx3RnHk&t=1028s&pp=ugMICgJrbxABGAE%3D" target="_blank">WDS 설명</a>
