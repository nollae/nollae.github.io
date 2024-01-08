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
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
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

![img02](/assets/img/posts/front/20231221_35_02.png){: .w-100 .h-75 .rounded-10 } 

![img03](/assets/img/posts/front/20231221_35_03.png){: .w-100 .h-75 .rounded-10 } 
_Bootstrap Breakpoint_

.col-- class가 선언된 요소는 **viewport width에 따라 수평정렬 상태에서 수직정렬 상태로 변경**된다. 이는 class가 적용 여부에 따라 결정되는데 *class가 적용되면 수평정렬 상태를 유지*한다.

`col-xs-\*(~ 768px)` 클래스가 선언된 요소의 경우, **viewport width에 관계없이 언제나 class가 적용되어 수평정렬 상태를 유지**하지만, `col-sm-\*`, `col-md-\*`, `col-lg-\*` 클래스가 선언된 요소의 경우, **viewport width에 따라 class가 적용 여부가 결정되고 이에 따라 수평정렬 상태에서 수직정렬 상태로 변경**된다.

| viewport width |  class  | class 적용 여부 | behavior |
|:---------------|:--------|:--------------|:---------|
| 300px의 경우 | col-xs | ◯ | 수평정렬 |
|            | col-sm | ✕ | stack |
|            | col-md | ✕ | stack |
|            | col-lg | ✕ | stack |
| 800px의 경우 | col-xs | ◯ | 수평정렬 |
|            | col-sm | ◯ | 수평정렬 |
|            | col-md | ✕ | stack |
|            | col-lg | ✕ | stack |
| 1000px의 경우 | col-xs | ◯ | 수평정렬 |
|            | col-sm | ◯ | 수평정렬 |
|            | col-md | ◯ | 수평정렬 |
|            | col-lg | ✕ | stack |
| 1500px의 경우 | col-xs | ◯ | 수평정렬 |
|            | col-sm | ◯ | 수평정렬 |
|            | col-md | ◯ | 수평정렬 |
|            | col-lg | ◯ | 수평정렬 |

Bootstrap의 그리드 시스템은 **12열까지 지원**한다. 두번째 \*에는 1부터 12까지의 숫자 중의 하나를 지정한다.

예를 들어 col-xs-1은 행 너비의 1/12를 열의 너비로 한다는 의미이다. col-xs-6은 행 너비의 6/12를 열의 너비로, col-xs-12은 행 너비의 12/12를 열의 너비로 지정한다.

<span class="bg-yllw">**즉 col-xs-12는 전체 너비, col-xs-6은 전체 너비의 반을 의미한다.**</span>

col-xs-1의 경우, 행에 12개가 들어 올 수 있으며 col-xs-6의 경우 2개, col-xs-12의 경우 1개가 들어 올 수 있다.

즉 행에 들어 올 수 있는 열은 두번째 \*의 합 만큼이다. <u>12보다 클 경우는 다음 줄로 넘어간다.</u>

- 두번째 \*의 합이 12인 경우, 행의 너비를 꽉 채운다.
- 두번째 \*의 합이 12보다 작은 경우, 오른쪽에 남는 부분이 생긴다.
- 두번째 \*의 합이 12보다 큰 경우, 12를 넘게한 마지막 열이 다음 줄로 넘어간다.

#### **2.1 .col-xs\* class**

<span class="bg-yllw">**viewport width와 관계없이 `.col-xs-*` 클래스는 언제나 적용되어 `.col-xs-*` 틀래스가 선언된 요소는 항상 수평으로 정렬된다.**</span>

아래는 Bootstrap에 정의되어 있는 `.col-xs-*` 클래스의 룰셋이다.

```css
.col-xs-1 {
  width: 8.33333333%;
}

.col-xs-2 {
  width: 16.66666667%;
}

/* .col-xs-3 ~ .col-xs-12 */

.col-xs-1, .col-xs-10, .col-xs-11, .col-xs-12, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9 {
  float: left;
}

.col-lg-1, .col-lg-10, .col-lg-11, .col-lg-12, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-md-1, .col-md-10, .col-md-11, .col-md-12, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-sm-1, .col-sm-10, .col-sm-11, .col-sm-12, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-xs-1, .col-xs-10, .col-xs-11, .col-xs-12, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9 {
  position: relative;
  min-height: 1px;
  padding-right: 15px;
  padding-left: 15px;
}
```

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <p>viewport 너비와 관계없이 항상 수평으로 정렬된다.</p>
    <div class="row">
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
    </div>
    <div class="row">
      <div class="col-xs-8">xs-8</div>
      <div class="col-xs-4">xs-4</div>
    </div>
    <div class="row">
      <div class="col-xs-5">xs-5</div>
      <div class="col-xs-5">xs-5</div>
    </div>
    <div class="row">
      <div class="col-xs-5">xs-5</div>
      <div class="col-xs-5">xs-5</div>
      <div class="col-xs-4">xs-4</div>
    </div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html 
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <p>viewport 너비와 관계없이 항상 수평으로 정렬된다.</p>
    <div class="row">
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
      <div class="col-xs-1">xs-1</div>
    </div>
    <div class="row">
      <div class="col-xs-8">xs-8</div>
      <div class="col-xs-4">xs-4</div>
    </div>
    <div class="row">
      <div class="col-xs-5">xs-5</div>
      <div class="col-xs-5">xs-5</div>
    </div>
    <div class="row">
      <div class="col-xs-5">xs-5</div>
      <div class="col-xs-5">xs-5</div>
      <div class="col-xs-4">xs-4</div>
    </div>
  </div>
</body>
</html>
' %}

#### **2.2 .col-sm-\* class**

<span class="bg-yllw">**viewport width가 768px 이상(768px ~)일 때 `.col-sm-*` class는 적용된다.**</span> <span class="bg-yllw">**768px 미만일 때는**</span> media query에 의해 `.col-sm-*` class가 적용되지 않고 <span class="bg-yllw">**div 요소의 block 특성에 의해 100%의 너비를 가지며 수직으로 쌓이게 된다.**</span>

아래는 Bootstrap에 정의되어 있는 `.col-sm-*` 클래스의 룰셋이다.

```css
@media (min-width: 768px) {
  .col-sm-1 {
    width: 8.33333333%;
  }

  .col-sm-2 {
    width: 16.66666667%;
  }

  /* .col-sm-3 ~ .col-sm-12 */

  .col-sm-1, .col-sm-10, .col-sm-11, .col-sm-12, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9 {
    float: left;
  }
}

.col-lg-1, .col-lg-10, .col-lg-11, .col-lg-12, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-md-1, .col-md-10, .col-md-11, .col-md-12, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-sm-1, .col-sm-10, .col-sm-11, .col-sm-12, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-xs-1, .col-xs-10, .col-xs-11, .col-xs-12, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9 {
  position: relative;
  min-height: 1px;
  padding-right: 15px;
  padding-left: 15px;
}
```

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <p>viewport width가 768px 이상(768px ~)일 때 .col-sm-* class는 적용된다. 768px 미만일 때는 media query에 의해 .col-sm-* class가 적용되지 않고 div 요소의 block 특성에 의해 100%의 너비를 가지며 수직으로 쌓이게 된다.</p>
    <div class="row">
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
    </div>
    <div class="row">
      <div class="col-sm-8">sm-8</div>
      <div class="col-sm-4">sm-4</div>
    </div>
    <div class="row">
      <div class="col-sm-5">sm-5</div>
      <div class="col-sm-5">sm-5</div>
    </div>
    <div class="row">
      <div class="col-sm-5">sm-5</div>
      <div class="col-sm-5">sm-5</div>
      <div class="col-sm-4">sm-4</div>
    </div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html 
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <p>viewport width가 768px 이상(768px ~)일 때 .col-sm-* class는 적용된다. 768px 미만일 때는 media query에 의해 .col-sm-* class가 적용되지 않고 div 요소의 block 특성에 의해 100%의 너비를 가지며 수직으로 쌓이게 된다.</p>
    <div class="row">
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
      <div class="col-sm-1">sm-1</div>
    </div>
    <div class="row">
      <div class="col-sm-8">sm-8</div>
      <div class="col-sm-4">sm-4</div>
    </div>
    <div class="row">
      <div class="col-sm-5">sm-5</div>
      <div class="col-sm-5">sm-5</div>
    </div>
    <div class="row">
      <div class="col-sm-5">sm-5</div>
      <div class="col-sm-5">sm-5</div>
      <div class="col-sm-4">sm-4</div>
    </div>
  </div>
</body>
</html>
' %}

#### **2.3 .col-md-\* class**

viewport width가 992px 이상(992px ~)일 때 `.col-md-*`class는 적용된다. 992px 미만일 때는 media query에 의해 `.col-md-*` class가 적용되지 않고 div 요소의 block 특성에 의해 100%의 너비를 가지며 수직으로 쌓이게 된다.

#### **2.3 .col-lg-\* class**

viewport width가 1200px 이상(1200px ~)일 때 `.col-lg-*` class는 적용된다. 1200px 미만일 때는 media query에 의해 `.col-lg-*` class가 적용되지 않고 div 요소의 block 특성에 의해 100%의 너비를 가지며 수직으로 쌓이게 된다.

### **3. col- class의 복합 구성**

지금까지는 하나의 요소에 하나의 Class prefix(.col-xs-, .col-sm-, .col-md-, .col-lg-)만을 사용하였다.

```html
<div class="col-sm-8">sm-8</div>
```

<span class="bg-yllw">하지만 **Class prefix를 혼합하여 사용할 수도 있다.**</span>

```html
<div class="col-xs-12 col-sm-6">xs-12 sm-6</div>
```

위와 같이 정의하면 아래와 같이 동작한다.

- viewport 너비가 768px 미만(~ 768px)이면 `.col-xs-12` class가 적용된다.
- viewport 너비가 768px 이상(768px ~)이면 `.col-sm-6` class가 적용된다.
하지만 `col-xs-*` class는 언제나 적용된다고 하였다.

viewport width가 768px 이상(768px ~)인 경우 요소에 지정된 두개의 클래스는 경합하게 된다. 이때 <span class="bg-yllw">**우선순위는 CSS 파일 내에서 후위에 선언된 룰셋이 더 높다.**</span> 따라서 CSS 파일(bootstrap.css) 내에서 **`.col-xs-12`보다 후위에 정의된 `.col-sm-6`가 적용되게 된다.**

```css
/* Extra small devices (phones, less than 768px) */
/* Rule set for col-sm-* class */

/* Small devices (tablets, 768px and up) */
@media (min-width: 768px) {
  /* Rule set for col-sm-* class */
}
/* Medium devices (desktops, 992px and up) */
@media (min-width: 992px) {
  /* Rule set for col-md-* class */
}
/* Large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
  /* Rule set for col-lg-* class */
}
```

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .blue { color: blue; }
    .red  { color: red; }
  </style>
</head>
<body>
  <div class="red blue">Text</div>
</body>
</html>
```

<span class="bg-yllw">**breakpoint에 해당하는 class가 선언되어 있지 않다면 하위 breakpoint에 해당하는 class가 적용된다.**</span>

```html
<div class="row">
  <div class="col-sm-12 col-md-8">.col-sm-12 .col-md-8</div>
  <div class="col-sm-6">.col-sm-6</div>
</div>
```

위의 경우, viewport width가 992px 이상일 때 첫번째 div 요소는 `col-md-8`가 지정되어 있으므로 `col-md-8` class가 적용되지만 두번째 div 요소에는 `col-md-`이 지정되어 있지 않다. 따라서 viewport 너비가 992px 이상이더라도 `col-sm-6` class가 적용된다.

![img02](/assets/img/posts/front/20231221_35_03.png){: .w-100 .h-15 .rounded-10 }

이는 `col-xs-*` class를 제외한 모든 `col-*-` class의 media query 조건이 min-width로 설정되었기 때문이다. *min-width는 프로퍼티값 이상을 의미한다.* 예를 들어 min-width: 768px의 경우 viewport 너비가 768px 이상일 경우 적용된다.

#### **3.1 Mobile and desktop**

Bootstrap의 breakpoint는 기본적으로 아래와 같다.

- Mobile의 경우 breakpoint는 768px 미만(~ 768px)이며 class prefix는 `.col-xs-*`이다.
- Desktop의 경우, breakpoint는 992px(992px ~) 이상이며 class prefix는 `.col-md-*`이다.
`.col-xs-*` class와 `.col-md-*` class를 혼용하면 아래와 같이 동작한다.

- viewport width가 992px 미만(~ 768px)이면 `.col-xs-*` class가 적용된다.
- viewport width가 992px 이상(992px ~)이면 `.col-md-*` class가 적용된다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row {
      margin-bottom: 10px;
    }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <p>Viewport width가 992px 이상이면 2열, 미만이면 1열로 정렬된다</p>
    <div class="row">
      <!--
      viewport width가 992px(medium device) 이상이면
      .col-md-8 class가 적용되어 width는 8/12(66.66666667%)

      viewport width가 992px(medium device) 미만이면
      .col-xs-12 class가 적용되어 width는 12/12(100%)
      -->
      <div class="col-xs-12 col-md-8">.col-xs-12 .col-md-8</div>
      <!--
      viewport width가 992px(medium device) 이상이면
      .col-md-4 class가 적용되어 width는 4/12(33.33333333%)

      viewport width가 992px(medium device) 미만이면
      .col-xs-12 class가 적용되어 width는 12/12(100%)
      -->
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
    </div>

    <p>Viewport width가 992px 이상이면 3열, 미만이면 1열로 정렬된다</p>
    <div class="row">
      <!--
      viewport width가 992px(medium device) 이상이면
      .col-md-4 class가 적용되어 width는 4/12(33.33333333%)

      viewport width가 992px(medium device) 미만이면
      .col-xs-12 class가 적용되어 width는 12/12(100%)
      -->
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
    </div>

    <p>viewport width와 관계없이 2열로 정렬된다</p>
    <div class="row">
      <!--
      viewport width와 관계없이 .col-xs-6 class가 적용되어 width는 6/12(50%)
      -->
      <div class="col-xs-6">.col-xs-6</div>
      <div class="col-xs-6">.col-xs-6</div>
    </div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html 
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row {
      margin-bottom: 10px;
    }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <p>Viewport width가 992px 이상이면 2열, 미만이면 1열로 정렬된다</p>
    <div class="row">
      <!--
      viewport width가 992px(medium device) 이상이면
      .col-md-8 class가 적용되어 width는 8/12(66.66666667%)

      viewport width가 992px(medium device) 미만이면
      .col-xs-12 class가 적용되어 width는 12/12(100%)
      -->
      <div class="col-xs-12 col-md-8">.col-xs-12 .col-md-8</div>
      <!--
      viewport width가 992px(medium device) 이상이면
      .col-md-4 class가 적용되어 width는 4/12(33.33333333%)

      viewport width가 992px(medium device) 미만이면
      .col-xs-12 class가 적용되어 width는 12/12(100%)
      -->
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
    </div>

    <p>Viewport width가 992px 이상이면 3열, 미만이면 1열로 정렬된다</p>
    <div class="row">
      <!--
      viewport width가 992px(medium device) 이상이면
      .col-md-4 class가 적용되어 width는 4/12(33.33333333%)

      viewport width가 992px(medium device) 미만이면
      .col-xs-12 class가 적용되어 width는 12/12(100%)
      -->
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
      <div class="col-xs-12 col-md-4">.col-xs-12 .col-md-4</div>
    </div>

    <p>viewport width와 관계없이 2열로 정렬된다</p>
    <div class="row">
      <!--
      viewport width와 관계없이 .col-xs-6 class가 적용되어 width는 6/12(50%)
      -->
      <div class="col-xs-6">.col-xs-6</div>
      <div class="col-xs-6">.col-xs-6</div>
    </div>
  </div>
</body>
</html>
' %}

#### **3.2 Mobile, tablet, desktop**

Bootstrap의 breakpoint는 기본적으로 아래와 같다.

- Mobile의 경우 breakpoint는 768px 미만이며 class prefix는 `.col-xs-*`이다.
- tablet의 경우 breakpoint는 768px 이상이며 class prefix는 `.col-sm-*`이다.
- Desktop의 경우, breakpoint는 992px 이상이며 class prefix는 `.col-md-*`이다.

`.col-xs-*` class, `.col-sm-*` class, `.col-md-*` class를 혼용하면 아래와 같이 동작한다.

- viewport width가 768px 미만이면 `.col-xs-*` class가 적용된다.
- viewport width가 768px 이상이면 `.col-sm-*` class가 적용된다.
- viewport width가 992px 이상이면 `.col-md-*` class가 적용된다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row {
      margin-bottom: 10px;
    }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
    p.bg-info {
      font-weight: bold;
      padding: 10px;
      margin-right: -15px;
      margin-left: -15px;
    }
  </style>
</head>
<body>
  <div class="container">
    <p class="bg-info">Viewport width가 992px 이상이면 3열, 768px~991px이면 2열, 768px미만이면 1열로 정렬된다</p>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-md-4">1</div>
      <div class="col-xs-12 col-sm-6 col-md-4">2</div>
      <div class="col-xs-12 col-sm-6 col-md-4">3</div>
      <div class="col-xs-12 col-sm-6 col-md-4">4</div>
      <div class="col-xs-12 col-sm-6 col-md-4">5</div>
      <div class="col-xs-12 col-sm-6 col-md-4">6</div>
    </div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html 
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row {
      margin-bottom: 10px;
    }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
    p.bg-info {
      font-weight: bold;
      padding: 10px;
      margin-right: -15px;
      margin-left: -15px;
    }
  </style>
</head>
<body>
  <div class="container">
    <p class="bg-info">Viewport width가 992px 이상이면 3열, 768px~991px이면 2열, 768px미만이면 1열로 정렬된다</p>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-md-4">1</div>
      <div class="col-xs-12 col-sm-6 col-md-4">2</div>
      <div class="col-xs-12 col-sm-6 col-md-4">3</div>
      <div class="col-xs-12 col-sm-6 col-md-4">4</div>
      <div class="col-xs-12 col-sm-6 col-md-4">5</div>
      <div class="col-xs-12 col-sm-6 col-md-4">6</div>
    </div>
  </div>
</body>
</html>
' %}

**breakpoint에 해당하는 class가 선언되어 있지 않다면 하위 breakpoint에 해당하는 class가 적용된다.**

### **4. Nesting columns**

열 내부에 그리드를 추가하면 **자식 그리드의 전체 너비는 부모 열의 너비와 같다.**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-9">
        Level 1: .col-sm-9
        <div class="row">
          <div class="col-xs-8 col-sm-6">
            Level 2: .col-xs-8 .col-sm-6
          </div>
          <div class="col-xs-4 col-sm-6">
            Level 2: .col-xs-4 .col-sm-6
          </div>
        </div>
      </div>
    </div>
  </div>
</body>
```

> **Result:** 

{% include embed/example.html height='200px'
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-9">
        Level 1: .col-sm-9
        <div class="row">
          <div class="col-xs-8 col-sm-6">
            Level 2: .col-xs-8 .col-sm-6
          </div>
          <div class="col-xs-4 col-sm-6">
            Level 2: .col-xs-4 .col-sm-6
          </div>
        </div>
      </div>
    </div>
  </div>
</body>
' %}

### **5. Offsetting columns**

열에 `.col-*-offset-*` class를 추가하면 오른쪽으로 열을 이동시킬 수 있다.

예를 들어 `<div class="col-md-2 col-md-offset-4">`의 경우, viewport 너비가 992px 이상이면 .col-md-4 만큼 오른쪽으로 이동한 후 .col-md-2 만큼의 너비를 갖는 열을 표시한다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <div class="col-xs-4">.col-xs-4</div>
      <div class="col-xs-4 col-xs-offset-4">.col-xs-4 .col-xs-offset-4</div>
    </div>
    <div class="row">
      <div class="col-xs-3 col-xs-offset-3">.col-xs-3 .col-xs-offset-3</div>
      <div class="col-xs-3 col-xs-offset-3">.col-xs-3 .col-xs-offset-3</div>
    </div>
    <div class="row">
      <div class="col-xs-6 col-xs-offset-3">.col-xs-6 .col-xs-offset-3</div>
    </div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html height='220px'
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <div class="col-xs-4">.col-xs-4</div>
      <div class="col-xs-4 col-xs-offset-4">.col-xs-4 .col-xs-offset-4</div>
    </div>
    <div class="row">
      <div class="col-xs-3 col-xs-offset-3">.col-xs-3 .col-xs-offset-3</div>
      <div class="col-xs-3 col-xs-offset-3">.col-xs-3 .col-xs-offset-3</div>
    </div>
    <div class="row">
      <div class="col-xs-6 col-xs-offset-3">.col-xs-6 .col-xs-offset-3</div>
    </div>
  </div>
</body>
</html>
' %}

### **6. Column ordering**

`.col-*-push-*` 와 `.col-*-pull-*` 클래스를 사용하여 열의 순서를 변경할 수 있다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <div class="col-xs-9 col-xs-push-3">.col-xs-9 .col-xs-push-3</div>
      <div class="col-xs-3 col-xs-pull-9">.col-xs-3 .col-xs-pull-9</div>
    </div>
  </div>
</body>
</html>
```

> **Result:** 

{% include embed/example.html height='120px'
content='
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
  <style>
    .row { margin-bottom: 10px; }
    [class|="col"] {
      background: #2db34a;
      border: 1px solid #eaeaed;
      height: 50px;
      font-size: .8em;
      line-height: 50px;
      text-align: center;
      color: white;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <div class="col-xs-9 col-xs-push-3">.col-xs-9 .col-xs-push-3</div>
      <div class="col-xs-3 col-xs-pull-9">.col-xs-3 .col-xs-pull-9</div>
    </div>
  </div>
</body>
</html>
' %}



<br/>

🔗 **ref :** 

- [**Bootstrap 그리드 시스템**](https://poiemaweb.com/bootstrap-grid-system)


