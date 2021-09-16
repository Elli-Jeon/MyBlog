---
title: "[React] Login과 관련된 개념"
tags: ["react"]
published: true
date: "2021-08-21"
---

# 로그인 프로세스를 이해하는데에 필요한 개념 정리

## 1. 쿠키, 세션, 캐시, 스토리지

- 쿠키 : 컴퓨터에 저장되는 작은 기록, 정보 파일 (개인 소장용 방문일지)
- 세션 : 서버쪽에 저장되는 정보 파일
- 웹 캐시 : 이미지 같은 용량이 큰 파일을 그때그때 서버에서 가져오면 부하가 심하니 따로 공간을 마련해서 컴퓨터에 저장

- 로컬 스토리지, 세션 스토리지 : 둘 다 HTML 5에 도입된 간단한 키와 값을 저장할 수 있는 저장소. window 객체로 접근 가능

쿠키와 세션에 대한 추가 정보!!

1. 쿠키는 client가 아니라 server쪽에서 정해서 보내주는 정보. 이 정보를 client에서 저장해두었다가 통신이 이루어질 때마다 자동으로 request header에 들어가서 서버에 전송 됨
2. 세션은 서버 메모리로 저장되지만 세션 역시 클라이언트에 쿠키로 저장(세션 id)

|               |               로컬스토리지                |      세션스토리지       |
| ------------- | :---------------------------------------: | :---------------------: |
| 데이터 영구성 |      사용자가 지우지 않는 이상 영구       | 윈도우, 탭 닫을 시 제거 |
| 사용 방법     |                자동 로그인                |      일회성 로그인      |
| 주의 사항     | 비밀번호와 같은 중요한 정보는 절대 저장 X |

---

## 2. 로그인 정보의 저장

- 절대 로컬 스토리지, 세션 스토리지에는 jwt token이 저장되면 안됨. XSS 공격에 매우 취약
- jwt token을 하나만 사용할 경우 이를 탈취하여 사용 가능

따라서 access token, refresh token 두 가지의 토큰을 생성해내고 refresh token을 secure, httpOnly 쿠키로, access token을 json body로 보내서웹 앱 내 로컬변수로 저장하여 사용

클라이언트는 set cookie에 http only 태그가 달려있어서 접근이 불가능. 요 쿠키는 특정 api와 통신시에 자동으로 달림(?) 그렇기에 요 토큰이 만료되기 전까지는 계속 통신하면서 access token을 header bearer에 담아 보내면 됨

<a href="https://velog.io/@yaytomato/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%90%EC%84%9C-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0" target="_blank">전체 프로세스 참조 사이트 1</a>

<a href="https://tansfil.tistory.com/59">token에 대한 참조 사이트 2</a>

## 3. 정리

정리를 하다가 도저히 이해가 가지 않아 라매개발자님에게 여쭈어 보았다.

> refresh token과 access token을 이용해서 로그인을 한다는 점은 알겠는데 로그인 유지는 어떻게 하나?

내가 생각한 방법은

1. access token을 local storage에 저장해서 유지.

->이 방법은 access token과 refresh token을 나누는 이유가 사라져 보였습니다.

2. 페이지가 새로 고침될 때마다 useEffect로 api에 axios로 로그인 상태 확인을 요청하여 isLoggedIn state를 true false로 관리

->이러면 과도한 request요청이 될 것 같았습니다.

3. redux-persist로 로그인 여부를 전역 관리

->페이지가 새로고침되어도 state가 유지되나 만료 시간을 설정할 수 있는지가 의문이 들었습니다.

4. access token을 단순 로컬 변수로만 사용

->다른 컴포넌트에서 사용이 힘들겠다는 생각이 들었습니다.

그래서 온 답변은 다음과 같다.

1. 이 왜 나누는 이유가 없어 보이시나요?? 1로 사용하다 액세스토큰 만료되면 refresh token 사용해서 다시 access token 받아오면 됩니다 refresh token이 없으면 다시 아이디 패스워드 입력해서 받아오니까 더 불편하겠죠?
   저희 회사는 1번 씁니다!

2. 는 새로고침할 때 마다 access token값을 새로 받아와야한다면 계속 아이디 비밀번호 입력을 해야해서 적절하진 않아 보여요

3. 만료시간은 서버가 토큰 발급할 때 토큰에 만료시간을 넣어줍니다. 프론트가 어떻게 됐던지간에 만료시간은 상관없어보이는데 그렇게 생각하신 다른 이유가 있을까요?

4. 정말 보안이 중요하다면 그렇게 해도 되겠죠??

결론은 엑세스 토큰을 로컬 스토리지에 넣어두고 사용한다고 한다. 어차피 리프레쉬 토큰은 http only라서 접근이 안되니 1시간 마다 만료되는 엑세스토큰을 사용해서 현재 로그인한 유저를 체크한다고 한다. 아마 토큰이 있다면 로그인한 유저를 체크하는 hook을 운용하지 않을까..?? 싶다.

## 4. + 21/09/13 추가

---

공부를 더 하다가 이제는 어느정도 개념을 잡은 것 같아서 추가한다.

1. access token과 refresh token으로 나누어서 token을 관리한다. access token은 10분 내외, refresh token은 2주 정도로 만료기간을 잡아 둔다.

2. client에서 로그인을 시도하면 서버는 아이디와 비밀번호를 확인해서 response를 보낸다. 이 response에는 access token이 response.data(JSON payload)에, refresh token이 HttpOnly, secure 태그를 단 쿠키로 넘어온다.

3. client 쪽에서는 이 access token과 refresh token을 보관해야한다. refresh token은 쿠키이므로 자동으로 앞으로의 HTTP 통신에 따라 붙는다. (경우에 따라 local storage에 저장도 되는데 이는 아래 링크 참조) 문제는 access token인데 이 access token을 앞으로 request Header의 Authorization에 넣어야 한다.

4. 찾아보기로는 다음과 같이 하면 된다고 한다.

```javascript
// saga에서 access token 받고
axios.defaults.headers.common["Authorization"] = `Bearer ${accessToken}`
```

5. 문제는 나는 아무리 해도 이게 안 된다는 것..! 그래서 access token을 local storage에 저장하는 것도 고려해보았으나 아무래도 보안의 우려가 있었다. 따라서 결정한 방법이 *Auth class*화 였다. auth api를 모아둔 것을 Auth 클래스로 만들어서 거기에 access token을 보관하였다. (추후 클래스 내 private 한 프로퍼티로 만들 생각)

> 09.14 추가 : 위 5번의 방식이 되지만 문제는 auth를 class 화해서 내부에 access token을 넣으려 했는데 page refresh를 하면 초기화하는거는 매한가지였다... 그래서 local storage에 넣어두고 페이지가 refresh될 때마다 access token을 새로 발급하기로 하였다. 

6. 이렇게 하면 문제가 다 해결된다. acces token은 private하게 js variable로 관리가 가능하고, refresh token은 http only인 쿠키이다.

7. axios instance에서 interceptor로 header에 access token을 달아두므로 인증이 필요한 작업을 할 수 있게 된다.

8. 추가로 로그인 유지는 App 에다가 (최상단 컴포넌트) 다음과 같은 작업을 해준다. refresh 요청을 보낼 때마다 백엔드에서는 새로운 access token을 지급해준다.

> 09.16 추가 : 로그인 유지에 대해서 중대한 변경(?) 사항이 생겼다. 기존에는 redux-persist를 이용해서 isLoggedIn을 true false로 나누어서 관리를 하였다. 목적은 UI 상으로 보여지는 것 관리. 여기서 또 문제가 발생하는데, 이렇게 되면 화면에 보여지는 로그인 여부와 실제 로그인 여부 사이에 괴리감이 생긴다는 것이다. 따라서 라매개발자님의 작업과 facebook을 뒤져 봤는데 결론은 다음과 같다. 

9. App component에 다음의 작업을 추가해준다. silent refresh 뿐만 아니라 유저의 로그인 여부를 확인하는 그리고 누가 로그인 했는지에 대한 정보를 refresh token을 통해서 비교하고 받아오는 작업을 app을 켜자마자 작동시키고 (useEffect를 통해서) 작업이 진행되는 동안 스피너 / 회색 화면 등을 띄어서 로그인이 안 된 상태 => 로그인이 된 상태로 화면이 깜빡이는 것을 막는다. (이는 react-router의 페이지 이동은 새로고침이 일어나지 않는 작업이기 때문이다.)

10. silent refresh와 현재 로그인 유저 체크는 refresh 토큰만을 이용한다. 나중에 글쓰기 등의 작업은 access token을 확인한다. 만약 없다? 그럼 로그인 한 것처럼 보여도 재인증절차를 거쳐야 한다.

11. remember me? 의 경우 refresh token의 만료 시간을 설정함으로서 해결할 수 있는데, 백엔드에서 전송하는 쿠키에 expire time을 설정하지 않는다면 브라우저 창을 닫으면 브라우저의 쿠키에서 자동 삭제된다고 한다.

```javascript
// app.js
useEffect(()=>{
   const timer = 시간;
   setTimeout(silentRefresh(), timer - 6000);
}, []);
// 페이지가 refresh 될때마다 silent refresh 실행, 타이머도 두어서 시간마다 체크

silentRefresh는 access token을 단 채로 api요청. response에다가 access token을 새로 매달아서, 이를 변경해주어야 함.
```

9. 로그아웃은 refresh token을 쿠키에서 지워주고, 백엔드에서도 db에 저장해둔 refresh token을 지워준다.

[JWT는 어디서 보관해야할까?](https://velog.io/@0307kwon/JWT%EB%8A%94-%EC%96%B4%EB%94%94%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%B4%EC%95%BC%ED%95%A0%EA%B9%8C-localStorage-vs-cookie)
