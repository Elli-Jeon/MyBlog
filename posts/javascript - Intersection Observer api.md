---
title: "[Javascript] Intersection Observer API (ft.react)"
tags: ["javascript", "react"]
published: true
date: "2021-09-13"
---

<h1 align="center">Intersection Observer API (ft.react)</h1>

## 1. 개요

라매개발자님이 주최하신 팀 프로젝트 그룹에서 만난 팀원분들과 한 달여에 걸쳐 만든 여행지 소개 페이지 (_instagram 같은_)를 무사히(?) 만들어냈다! (사실 다 만든 것은 아니다... 오류도 많고 실사용자 피드백도 수정하지 않았으며 리팩토링하고 싶은 코드들도 많다.)

아무튼 나름의 마무리를 한 만큼 배우고, 기억해두고 싶은 것들을 정리해보기로 했다. 아직 다른 분들의 코드를 제대로 안 뜯어보았으므로 우선은 내가 이번에 배운 것을 하나 정리해보기로 했다.

> 이름하야 `Lazy Loading`!!!

인스타그램을 우리가 켜본다고 가정해보자. 정말 수 없이 많은 사진들이 내 컴퓨터 화면에 뜨게 될 것이다. 그런데 이 사진들을 그냥 한 번에 띄운다는 것은 매우 멍청한 생각이다. 첫 번째로는 시간이 오래 걸릴 것이며, 두 번째로는 트래픽이 너무 높아질 것이다. 그래서 등장한 아이디어가 바로 **Lazy Loading**이다. 사용자가 해당 사진에 접근하게 되면 (해당 아이템이 사용자의 화면에 나오게 되면) 그 때 서버에 사진을 요청하는 것이다!

이전에는 좀 다른 방법으로 lazy loading을 구현했다고 하는데... 그 방법이 너무 계산이 많아지고 그래서 효율이 안 좋다고 한다.; 그래서 나온 API가 바로 intersection observer API 다.

## 2. 사용방법

거두절미하고 사용 방법을 정리해보자. (react에서)

```javascript
// 해당 page 에서
...

const lazyTarget = useRef() //우리가 관측할 대상이다.

useEffect(()=>{
  let observer;

  if(lazyTarget.current){ // ref안에 관측할 컴포넌트가 있어야 시작

    // new IntersectionObserver(fn, option) 1. observer에 IO 넣어줌
    observer = new IntersectionObserver((entries)=>{
      entries.forEach((entry)=>{
        if(entry.intersecting){ // 화면과 대상이 겹쳐지면
          //할 거 함 (ex. 사진 불러오는 axios);
          //setTimeOut(()=>(setIsView(true)),1000);
          //setTimeOut은 isview의 true false에 따라 로딩 중인 fake image가 보이고 말고가 결정. false일 때는 보이다가 여기서 true로 바꿔주었음.
        }
      })
    },{})

    observer.observe(); // 2. 대상 관측 시작
  }

  return ()=> observer && observer.disconnect(); // useEffect cleanup 함수  // 3. useEffect 끝내면서 observer있으면 지워줌

},[])



...


<Component ref = {lazyTarget}/>

```
