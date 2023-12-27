---
title: Bootstrap 그리드 시스템
author: nollae
date: 2023-12-21 11:43:00 +0900
categories: [Front, Bootstrap]
tags: [Bootstrap]
---
<style>
    .bg-yllw { background-color: #fff5b1 }
    .bg-gray { background-color: #f6f8fa }
    .ft-small{ font-size: 0.9rem }
    blockquote { border-left-color: #F7DDBE }
</style>

<span class="ft-small">
해당 본문은 poiemaweb의 [**Bootstrap 그리드 시스템**](https://poiemaweb.com/bootstrap-grid-system) 를 필사하였다.
</span>

## **Media Query**

Bootstrap은 Mobile-first 방식을 기본 지원하므로 <span class="bg-yllw">**Media query에 포함되지 않은 모든 정의는 768px 미만 디바이스를 위한 것이다.**</span>

Bootstrap은 기본적으로 4개의 breakpoint로 구간을 나눈다.

```css
/* Extra small devices (phones, less than 768px) */
/* No media query since this is the default in Bootstrap */

/* Small devices (tablets, 768px and up) */
@media (min-width: @screen-sm-min) {
  /* ... */
}

/* Medium devices (desktops, 992px and up) */
@media (min-width: @screen-md-min) {
  /* ... */
}

/* Large devices (large desktops, 1200px and up) */
@media (min-width: @screen-lg-min) {
  /* ... */
}
```

@screen-*의 @는 [LESS](https://lesscss.org/)의 변수를 의미한다. LESS는 CSS 프리프로세서(Preprocessor)로서 Bootstrap의 소스코드는 LESS를 기반으로 작성되었다.

## **Container**

Bootstrap은 **콘텐츠를 감싸는 wrapping 요소(container)를 포함해야 한다.** container는 그리드 시스템의 필수 요소이다.

container에는 2가지 종류가 있다.

.container class
: fixed width container로서 **responsive fixed layout(반응형 고정폭 레이아웃)을 제공**한다.

.container-fluid class
: full width container로서 **fluid layout(유동 최대폭 레이아웃)을 제공**한다.

> 2가지 container를 중첩 사용해서는 안된다. padding에 문제가 발생하기 때문이다.
{: .prompt-warning}

### **1. fixed width container (responsive fixed layout)**

responsive fixed layout(반응형 고정폭 레이아웃)을 만들 때 사용한다. **Media query에 의해 반응형으로 동작하며 동일 breakpoint내에서는 viewport 너비가 늘어나거나 줄어들어도 고정폭을 갖는다.**

.container의 룰셋은 다음과 같다.

```css
.container {
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
/* Extra small devices (phones, less than 768px) */
/* No media query since this is the default in Bootstrap */

/* Small devices (tablets, 768px and up) */
@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}
/* Medium devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .container {
    width: 970px;
  }
}
/* Large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
  .container {
    width: 1170px;
  }
}
```

### **2. full width container (fluid layout)**

fluid layout(유동 최대폭 레이아웃)을 만들 때 사용한다. **viewport 너비에 상관없이 언제나 콘텐츠 요소를 화면에 꽉차는 너비를 갖게 한다.**

.container-fluid의 룰셋은 다음과 같다.

```css
.container-fluid {
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
```

다음은 fixed width container(container class)와 full width container(container-fluid class)이 예제이다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .container, .container-fluid {
      background: #eaeaed;
    }
    .fixed, .fluid {
      background: #2db34a;
      height: 100px;
      line-height: 100px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="fixed">fixed width (.container)</div>
  </div>
  <br>
  <div class="container-fluid">
    <div class="fluid">full width (.container-fluid)</div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html height="250px"
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .container, .container-fluid {
      background: #eaeaed;
    }
    .fixed, .fluid {
      background: #2db34a;
      height: 100px;
      line-height: 100px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="fixed">fixed width (.container)</div>
  </div>
  <br>
  <div class="container-fluid">
    <div class="fluid">full width (.container-fluid)</div>
  </div>
</body>
</html>
' %}

## **Grid system**

앞에서 설명한 .container와 .container-fluid는 콘텐츠 요소를 포함하는 부모 요소로서 container 또는 wrapping 요소라고 부른다. container는 그리드 시스템을 위한 필수 사항이다.

**그리드 시스템은 열을 나누어 콘텐츠를 원하는 위치에 배치하는 방법(Layout)**을 말한다. Bootstrap은 반응형 12열 그리드 시스템을 제공한다.

![img01](/assets/img/posts/front/20231221_35_01.png){: .w-75 .h-100 .rounded-10 } 
_Bootstrap Grid System_

> 그리드 레이아웃을 구성 시에는 반드시 `.row`(행)를 먼저 배치하고 행 안에 `.col-*-*`(열)을 필요한 갯수만큼 배치한다. 즉, container 내에 `.row`(행)을 먼저 배치하고 그 안에 `.col-*-*`(열)을 배치한다. 그리고 콘텐츠는 `.col-*-*` 내에 배치한다.
{: .prompt-info}

```console
.container or .container-fluid
  .row
    .col-xs-#
      contents
    .col-xs-#
      contents
  .row
    .col-xs-#
      contents
    .col-xs-#
      contents
```

### 1. 행(.row)의 구성

container(.container 또는 .container-fluid) 내에 `.row` class를 사용하여 행을 배치한다.

```html
<div class="container">
  <div class="row">
    <!-- ... -->
  </div>
  <div class="row">
    <!-- ... -->
  </div>
</div>
```

### **2. 열(.col-*-*)의 구성**

열은 행(.row) 내에 배치하여야 한다. .col-*-* class로 열을 배치하는데 첫번째 *에는 xs, sm, md, lg 중의 하나를 지정한다.

*Grid options*

| | Extra small devices Phones (~ 767px) | Small devices Tablets (768px ~) | Medium devices Desktops(992px ~) | Large devices Desktops (1200px ~) |
|:---|:---|:---|:---|:---|
| Grid behavior | 항상 수평 적용 | viewport 너비가 breakpoint 이상이면 수평 적용, 미만이면 stack |||

무엇이 문제일까?


<br/>

🔗 **ref :** 

- [**Bootstrap 그리드 시스템**](https://poiemaweb.com/bootstrap-grid-system)


