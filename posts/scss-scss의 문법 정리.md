---
title: '[CSS] SCSS에서 기억해야 할 문법'
tags: ["css"]
published: true
date: '2021-07-13'
---

SCSS에서 기억할만한 것들 정리
===

SCSS의 문법에 대해 이미 정리가 잘 되어 있는 페이지가 존재한다. 


><a href="https://heropy.blog/2018/01/31/sass/" target="_blank">SCSS 문법 총정리</a>

문법을 총망라해둔 사이트니 나중에 자세히 정독해보아야겠다. 여기에는 기억해야할 개념 몇가지를 적고 간다. 

---
### 1. 여러가지 기호 들

> @(함수), $(변수) , #{}(어디서나 변수 쓰게 해줌), &(부모 치환)

SCSS에는 다양한 기호가 쓰이는데 그 기호의 의미를 유추해보았다. 

### 2. Nesting

```scss
.container {
    $w : 10px;
    $h : 20px;
    weight : $w;
    height : $h;
    font {
        size : 1rem;
        weight : 400;
    }
    // 그 자식 p 에게 적용
    & p {
        color : red;
    }
    // hover 적용
    &:hover {
        background-color : red;
    }
    // .container-small 에 적용
    &-small {
        width : 50%;
        height : 50%;
    }
    // 해당 {} 안의 변수는 쓰고 싶은데 그 자식이 아닌 경우
    @at-root .box {
        width : $w;
    }
}
```

### 3. @mixin과 @include
```scss
@mixin fontSizeWeight($fontSize : 20px, $fontWeight : 400){
    font {
        size : $fontSize;
        weight : $fontWeight;
    }
}

@include fontSizeColor(18px, #fff);
```

### 4. if 문과 for문
```scss
@mixin fontWeight($fontWeight){
    font-weight : $fontWeight;
}

@mixin weight($font){
    @if $font == 'bold'{
        @include fontWeight(bold);
    } 
    @else if $font == 600 {
        @include fontWeight(600);
    }
    @else if $font == 400 {
        @include fontWeight(400);
    }
}

// for 반복문
@for $i from 1 through 5{
    .text-#{$i}{
        font-size : $i * 10px;
    }
}

```

그 외의 내용은 참조한 사이트를 참고하다.

<a href="https://heropy.blog/2018/01/31/sass/" target="_blank">SCSS 총정리</a>