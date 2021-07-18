---
title: '[React] redux의 작동 방식 (1)'
tags: ["react"]
published: true
date: '2021-07-18'
---

## Redux의 작동 구조
---

해당 글은 redux를 사용하려면 어떤 식으로 파일을 구성해야하는 지에 대해 기억하기 위해 쓰인 글인 만큼 구체적인 작동 방식은 다음의 포스트를 확인하는 것을 추천한다.

<a href="https://medium.com/@jsh901220/react%EC%97%90-redux-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0-a8e6efd745c9" target="_blank">redux 적용하기</a>

### 1. redux의 핵심 A.R.S

redux는 프로젝트 전반적인 상태 관리를 위해 사용한다. 어느 컴포넌트에서는 손 쉽게 접근, 수정이 가능하게 도와주는 라이브러리인 것이다. 

redux의 사용법 크게 4가지 파트로 나눠볼 수 있을 것 같다. 
- action
- reducer
- store
- (store를 사용하는)component

이 4가지 중 앞의 3개를 줄여서 내 맘대로 ARS라고 명칭을 지어봤다. 

자세한 것은 다음 그림을 통해 확인하자.

![](https://user-images.githubusercontent.com/68575268/126068976-5fe17092-2e80-4bb9-8bce-0f5760d036a1.png)

다음의 과정으로 redux를 통해 상태를 관리할 수 있다. 


1. action에서 action을 정의한다.
2. reducer에서 switch문을 통해 어떤 action에 어떤 일을 해줄 것인지 적어준다.
3. store에서 store을 열어주고, 거기에 reducer를 넣어준다. 나중에 middleware도 여기에 넣어준다.
4. App에서 provider를 씌어주고 props로 store를 자식 컴포넌트들에 내려준다.
5. 자식 컴포넌트에서 connect()함수로 store와 연결시켜준다. connect의 파라미터로 mapStateToProps와 mapDispatchToProps를 통해서 각각 store의 state를 내려주고, 자식 컴포넌트에서 dispatch로 어떤 action을 격발시킬지를 적어준다. 
 