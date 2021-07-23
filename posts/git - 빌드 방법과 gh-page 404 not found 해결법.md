---
tags:
  - ["git"]
published: true
date: 2021-07-04T17:09:22.708Z
title: '[FreeTalk] Starting Blog'
---

## 깃허브 페이지를 사용하여 빌드, 배포하는 법 (+404 에러 해결)
---

### 1. 컴파일, 빌드, 배포


얼마 전에 빌드라는 무엇일까? 하는 의문이 들어서 관련 내용을 찾아보던 중 신박한 비유를 하나 찾아서 공유를 해볼까 한다. 컴파일, 빌드, 배포를 비교하는 글이다.

> 컴파일 : 영문 책을 한글로 번역 (사용자의 언어 -> 컴퓨터의 언어)

> 빌드 : 번역한 책을 한 권으로 묶음 (실행할 수 있는 파일로 만듬)

> 배포 : 책을 서점에 진열 (사용자가 사용가능한 환경에 배치)

보통 컴파일부터 빌드를 거쳐 배포하는 과정을 *빌드한다*라고 부른다고 한다.

추가로, exe파일로 만드는 경우는 'distribution'이라 하고, 웹 환경에서는 'deploy'라고 한다고 한다.

자세한 내용은 다음 블로그를 참고하자.

<a href="https://itholic.github.io/qa-compile-build-deploy/" target="_blank">원문 블로그</a>

---

### 2. CRA에서 깃허브 페이지를 통한 빌드 방법

1. Add **homepage** to **package.json**

package.json에 홈페이지 주소를 적어준다.

```json
"homepage" : "http://[username].github.io/[appname]"
```

2. Install **gh-pages** and add **deploy** to **scripts** in **package.json**

```
yarn add gh-pages
```

```json
"scripts" : {
+   "predeploy" : "npm run build",
+   "deploy" : "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",    
}
```
\+ 표시가 된 두 줄을 추가한다. 

3. deploy site by running **npm run build**

```
npm run build
```
4. 깃허브에 들어가서 gh-pages branch로 설정해준다.

<a href="https://create-react-app.dev/docs/deployment/#github-pages" target="_blank">참고페이지</a>

## 3. 깃허브 페이지에서 새로고침하면 뜨는 404 에러 처리
---
이 문제의 경우 깃허브 페이지에서 HTML5의 history API를 쓰는 라우터를 지원하지 않아서 생기는 문제이다. 바로 위의 Docs에서도 설명이 있지만 한글로 설명이 된 페이지를 링크로 걸어둔다.

<a href="https://iamsjy17.github.io/react/2018/11/04/githubpage-SPA.html" target="_blank">참고페이지</a>

간단하게 적어두자면 404html을 우리가 만들어서 그 쪽으로 리다이렉션을 걸어두는 trick이다. 

### 1. public 폴더에 404.html 추가

```html
<!DOCTYPE html>
<html>
  <head>
      <meta charset="utf-8">
      <title>Single Page Apps for GitHub Pages</title>
      <script type="text/javascript">
          // Single Page Apps for GitHub Pages
          // https://github.com/rafrex/spa-github-pages
          // Copyright (c) 2016 Rafael Pedicini,  licensed under the MIT License
          //  ---------------------------------------------- ------------------------
          // This script takes the current url and  converts the path and query
          // string into just a query string, and then  redirects the browser
          // to the new url with only a query string  and hash fragment,
          // e.g. http://www.foo.tld/one/two?a=b& c=d#qwe, becomes
          // http://www.foo.tld/?p=/one/two&  q=a=b~and~c=d#qwe
          // Note: this 404.html file must be at least  512 bytes for it to work
          // with Internet Explorer (it is currently >  512 bytes)

          // If you're creating a Project Pages site  and NOT using a custom domain,
          // then set segmentCount to 1 (enterprise   users may need to set it to > 1).
          // This way the code will only replace the  route part of the path, and not
          // the real directory in which the app  resides, for example:
          //  https://username.github.io/repo-name/one/two?  a=b&c=d#qwe becomes
          // https://username.github.io/repo-name/? p=/one/two&q=a=b~and~c=d#qwe
          // Otherwise, leave segmentCount as 0.
          var segmentCount = 1;

          var l = window.location;
          l.replace(
              l.protocol + '//' + l.hostname + (l.port ?   ':' + l.port : '') +
              l.pathname.split('/').slice(0, 1 +  segmentCount).join('/') + '/?p=/' +
              l.pathname.slice(1).split('/').slice  (segmentCount).join('/').replace(/&/g,  '~and~') +
              (l.search ? '&q=' + l.search.slice(1) .replace(/&/g, '~and~') : '') +
              l.hash
          );

      </script>
  </head>
  <body>
  </body>
</html>
```

### 2. index.html 수정

404.html을 통해서 쿼리로 변경된 URL을 index.html에서 받는다. 

```html
<head>
...
  <!-- Start Single Page Apps for GitHub Pages -->
  <script type="text/javascript">
    // Single Page Apps for GitHub Pages
    // https://github.com/rafrex/spa-github-pages
    // Copyright (c) 2016 Rafael Pedicini, licensed under the MIT License
    // ----------------------------------------------------------------------
    // This script checks to see if a redirect is present in the query string
    // and converts it back into the correct url and adds it to the
    // browser's history using window.history.replaceState(...),
    // which won't cause the browser to attempt to load the new url.
    // When the single page app is loaded further down in this file,
    // the correct url will be waiting in the browser's history for
    // the single page app to route accordingly.
    (function (l) {
      if (l.search) {
        var q = {};
        l.search.slice(1).split('&').forEach(function (v) {
          var a = v.split('=');
          q[a[0]] = a.slice(1).join('=').replace(/~and~/g, '&');
        });
        if (q.p !== undefined) {
          window.history.replaceState(null, null,
            l.pathname.slice(0, -1) + (q.p || '') +
            (q.q ? ('?' + q.q) : '') +
            l.hash
          );
        }
      }
    }(window.location))
  </script>
  <!-- End Single Page Apps for GitHub Pages -->

</head>
```

3. basename 설정

프로젝트에서 라우팅으로 정해준 곳(App.js 혹은 index.js)에서 BrowserRouter에 basename을 정해준다.

예를 들어 package.json에
```json
"homepage" : "http://[username].github.io/home"
```
이라 했으면,

```javascript
//App.js

<Provider store={store}>
    <BrowserRouter basename="/home">
        <App />
    </BrowserRouter>
</Provider>
```

끝!