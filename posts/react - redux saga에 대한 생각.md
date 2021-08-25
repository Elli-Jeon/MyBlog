---
title: "[React] redx-saga의 필요성"
tags: ["react"]
published: true
date: "2021-08-25"
---


# redux-saga의 필요성과 그에 대한 생각 정리

## 1. redux-saga의 용도

- react : UI를 컴포넌트 기반으로 제작하고 애플리케이션의 상태를 화면으로 랜더링할 수 있게 해줌
- redux : 애플리케이션의 전역 상태를 효과적으로 공유하고 관리 /
UI -> Action -> Reducer -> State

> 문제 발생 !! "클릭 하면 api 받아오기" 같은 비동기 로직이 컴포넌트의 클릭 이벤트에 포함. 리액트 컴포넌트는 상태에 따른 화면 정의라는 본래의 의도를 벗어나서 비즈니스 로직을 포함하게 됨. **리액트 컴포넌트는 비즈니스 로직의 컨테이너가 아님**

Middleware를 사용 (dispatch를 감싸는 High Order Function) 

- redux-thunk : thunk는 dispatch를 품은 액션. 비동기처리를 해줌 

BUT. thunk 내부에 여러 개의 액션을 dispatch 해야 할 때, 하나의 액션이 여러가지 액션을 포함하게 됨, 기능의 작동 순서를 보장할 수 없고, 추적이 힘들고, action creator가 action object가 아니라 함수를 반환

- redux-saga : 제너레이터를 통해 액션의 순수성 보장

하나씩 하나씩 순서대로 처리할 수 있게 해줌. (ex- click을 여러번 했을 때, 각각의 기능이 순서대로 처리될 수 있게 도와줌)


> 웹앱의 상태를 출력하는 것은 react, 상태 관리는 redux, 외부 입력을 받아서 어떻게 처리할지에 대한 비즈니스 로직은 redux-saga 가 담당

<a href="https://min9nim.vercel.app/2020-04-23-redux-saga/" target="_blank">Redux-Saga가 해결해주는 문제</a>

## 2. redux-saga안의 Concept

### 1. side effect
```javascript
비동기 요청, 브라우저 캐시, 로컬 스토리지 ...
side effect (부수 효과) : (자바스크립트) 코드가 외부 세계에 영향을 주거나 받는 것!
```
### 2. generator 
```javascript
// Callee : generator function
function* myGeneratorFunction(){
  yield 1;
  yield 2;
  yield 3;
}

// Caller : Runner
const generator = myGeneratorFunction();

console.log(generator.next().value) // 1
console.log(generator.next().value) // 2
console.log(generator.next().value) // 3
```

### 3. saga
```javascript
saga == generator function 
redux-saga middleware == runner

generator는 side effect를 어떻게 처리하는지에 대한 description yield

redux-saga middleware는 실질적인 처리, 결과를 generator에 전달
```

### 4. effect
```javascript
redux의 action 같은 개념
effect is simply an object that contains some information to be interpreted by the middleware
```

> Saga는 Effect를 yield하고, Middleware는 Effect를 처리하여 결과를 돌려준다

![image](https://user-images.githubusercontent.com/68575268/130810476-4230f4d2-1917-415a-8ae6-d73a7eb3a6dc.png)


### 5. blocking effect VS non-blocking effect

![image](https://user-images.githubusercontent.com/68575268/130811291-37392a38-bf65-496e-9fcc-b714cfb83503.png)

### 6. channel
```javascript
web socket과 같은 외부 이벤트와 saga는 어떻게 연결하나??

websocket event는 push / saga는 pull 이라고 이해

일반화하여 Communicationg Sequential Processes

```
![image](https://user-images.githubusercontent.com/68575268/130812343-b4184e7c-4e1e-4a1e-93c3-35aabbefb67d.png)


![image](https://user-images.githubusercontent.com/68575268/130812448-3658144b-0447-407e-b083-6cb7385a59bc.png)

![image](https://user-images.githubusercontent.com/68575268/130812570-eb1ba5a8-665c-4ee5-8001-8c1a86e51f79.png)


<a href="https://www.youtube.com/watch?v=UxpREAHZ7Ck" target="_blank">Redux-Saga 개념 소개</a>


<a href="https://ssangq.netlify.app/posts/redux-saga" target="_blank">Redux-Saga의 간단한 사용법</a>

## 3. redux saga를 이용할 때 folder structure

reduxjs toolkit 의 도입 이후로, 더 이상 store 안에 module을 따로 빼놓고 pages 폴더 안에서 화면 별로 구성하기 보다는, features 안에 각 화면 별로 폴더를 만들고, 그 안에 index, slice, saga를 몰아 넣는다고 한다.  


![image](https://user-images.githubusercontent.com/68575268/130815585-6ce405c9-c6d2-44b8-9fa2-e847e444e73d.png)

<a href="https://im-developer.tistory.com/195" target="_blank">Redux-Saga 폴더 구조</a>


## 4. saga 파일 내 구조
```javascript
import { put, takeEvery, all } from 'redux-saga/effects'

const delay = (ms) => new Promise(res => setTimeout(res, ms))

function* helloSaga() {
  console.log('Hello Sagas!')
}

function* incrementAsync() {
  yield delay(1000)
  yield put({ type: 'INCREMENT' })
}

function* watchIncrementAsync() {
  yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}

// notice how we now only export the rootSaga
// single entry point to start all Sagas at once
export default function* rootSaga() {
  yield all([
    helloSaga(),
    watchIncrementAsync()
  ])
}

```
