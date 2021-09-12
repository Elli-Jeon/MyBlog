---
title: "[React] TDD의 필요성과 JEST"
tags: ["react"]
published: true
date: "2021-09-12"
---


# React에서 TDD의 필요성과 JEST & RTL

1. [TDD의 필요성](#1)
2. [JEST의 기본적인 사용법](#2)
3. [RTL의 기본적인 사용법](#3)


## 1. TDD의 필요성 <a id="1"></a>
---

이전에 정보처리기사 공부를 하면서 배웠던 프로그램 개방 방식에서 기억나는 방법이 두 가지가 있다. 하나는 *폭포수 방식 모델*로 전통적인 개발 방식이다. 다른 하나는 *애자일 방법론*으로 하나의 흐름을 따르는 폭포수 방식과는 다르게 그 때 그 때 필요한 요구를 더하고 수정하여 소프트웨어를 개발해나가는 방법을 의미한다. 

애자일 방법론에 TDD (Test Driven Development)라는 또 다른 개발 프로세스가 존재한다. 이는 Red - Green - Refactoring 이라는 그림을 보면 이해가 빠르다. [그림](https://www.google.com/url?sa=i&url=https%3A%2F%2Fmokacoding.com%2Fblog%2Fred-green-and-dont-forget-refactor%2F&psig=AOvVaw3tqt4RbJ7uLiYu_aUoGxck&ust=1631509236684000&source=images&cd=vfe&ved=0CAsQjRxqFwoTCKCe0NXT-PICFQAAAAAdAAAAABAD)

TDD는 두 가지 과정으로 나누어 볼 수 있다.

1. Make it Work 
2. Make it Clean

일단 테스트에 맞게 코드를 구현하고서 리팩토링을 하라는 것이다. 이 때, 리팩토링을 하면서 버그를 수정해서도 안 된다. 리팩토링은 말 그대로 기능을 그대로 유지한 채 좀 더 명확하게 코드를 수정하는 것이기 때문이다. 

그렇다면 테스트 기반으로 코딩을 하는 이유는 무엇일까?
내가 이해하기로는 다음과 같다. 

> 1. 내가 만들고자 하는 기능을 구현하는 것에 집중해서 코딩을 할 수 있게 된다.
> 2. 그 자체로 문서가 되어서 다른 개발자의 코딩을 뜯어볼 때 도움이 된다. 

## 2. JEST 란? <a id="2"></a>
---
JEST는 페이스북에서 출시한 테스팅 라이브러리로, '프레임워크'라고 부를 정도로 통합적인 기능을 제공한다.

```javascript
// package.json

"scripts" : {
  "test" : "jest
} 
```
로 설정하고서 `npm test`라고 해주면 자동으로 디렉토리 안의 `.test.js`로 끝나는 파일들을 모두 실행

### 1) 기본적인 사용법 
<br>

기본적인 테스트의 패턴은 다음과 같다.

```javascript
// test.js
test('테스트 설명',()=>{
  expect("검증대상").toXXX("기대결과");
})
```
`toXXX` 함수는 Test Matcher라고 한다. [공식문서 - Matchers](https://jestjs.io/docs/using-matchers)

다음은 많이 쓰이는 test matcher 함수들이다

```javascript
toBe : primitive 비교
toEqual : 객체 비교
toBeTruthy / toBeFalsy : true, false로 간주되는 타입 잡아줌
toHaveLength, toContain : 배열 길이 측정 / 특정 원소가 있는지 테스트
toMatch : 정규식 검증
```

```javascript
test("1 is 1", () => {
  expect(1).toBe(1)
})

function getUser(id){
  return {
    id,
    email : `user${id}@test.com`
  }
}

test("return a user object", ()=>{
  expect(getUser(1)).toEqual({
    id : 1,
    email : `user1@test.com`
  })
})

test("특정 color있는지 체크",()=>{
  const colors = ["red", "blue", "white"]
  expect(colors).toHaveLength(3);
  expect(colors).toContain("green");
  expect(colors).not.toBeNull();
})
--------------------------------------------------
> jest@1.0.0 test
> jest

 FAIL  ./test.js
  √ 1 is 1 (1 ms)
  √ return a user object (1 ms)
  × 특정 color있는지 체크 (3 ms)

  ● 특정 color있는지 체크

    expect(received).toContain(expected) // indexOf

    Expected value: "green"
    Received array: ["red", "blue", "white"]

      20 |   const colors = ["red", "blue", "white"]
      21 |   expect(colors).toHaveLength(3);
    > 22 |   expect(colors).toContain("green");
         |                  ^
      23 |   expect(colors).not.toBeNull();
      24 | })

      at Object.<anonymous> (test.js:22:18)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 2 passed, 3 total
Snapshots:   0 total
Time:        0.873 s, estimated 1 s
Ran all test suites.
```
`toThrow`로 에러 처리 검증을 할 때에는 다음과 같이 적어야 한다. 

```javascript
function getUser(id) {
  if (id <= 0) throw new Error("Invalid ID");
  return {
    id,
    email: `user${id}@test.com`,
  };
}


test("throw when id is non negative", () => {
  expect(() => getUser(-1)).toThrow();
  expect(() => getUser(-1)).toThrow("Invalid ID");
});
```

반드시 expect안에 함수로 넣어주어야 한다. 그러지 않으면 항상 실패만 나온다고 한다. 

### 2) 비동기 검증법 
<br>

```javascript
test("fetch user", async () => {
  const user = await fetchUser(1)
  expect(user).toEqual({
    id : 1,
    name : "user",
    email : "abc@test.com"
  })
})
```

출처 : [JEST 기본 사용법](https://www.daleseo.com/jest-basic/)

### 3) 그 외 사용법

```javascript
beforeEach, afterEach, beforeAll, afterAll, only, skip, describe, it
```
[테스트 전후 처리](https://www.daleseo.com/jest-before-after/)

[mocking fn, spyon](https://www.daleseo.com/jest-fn-spy-on/)

[mocking module](https://www.daleseo.com/jest-mock-modules/)

## 3. RTL?? <a id="3"></a>
---


[맨 위로](#)