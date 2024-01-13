---
title: Tabnabbing 피싱 공격의 동작 원리와 대응책
author: nollae
date: 2023-11-30 14:55:00 +0900
categories: [Front, Security]
tags: [Tabnabbing]
---
<style>
    .bg-yllw { background-color: #fff5b1 }
    .bg-gray { background-color: #f6f8fa }
    .ft-small{ font-size: 0.9rem }
    blockquote { border-left-color: #F7DDBE }
</style>

## **Tabnabbing 이란**

HTML 문서 내에서 링크(`target이 _blank`인 태그)를 클릭했을 때 **새롭게 열린 탭(또는 페이지)에서 기존의 문서 위치를 피싱 사이트로 변경해 정보를 탈취하는 공격 기술**을 말한다.

이 공격은 <u>메일이나 SNS와 같은 오픈 커뮤니티에서 사용</u> 될 수 있다. 사용자의 클릭을 유도하여 웹 브라우저의 탭을 피싱 사이트로 이동시키는 기존의 피싱 기법과 달리 탭내빙(Tabnabbing)은 사용자의 페이지에서 아무런 행위를 하지 않아도 사용자의 눈을 피해 열린 탭 중 하나를 피싱 페이지로 로드한다.

## **Tabnabbing 공격 절차**

![img01](/assets/img/posts/front/20231130_06_01.jpeg){: .w-100 .h-100 .shadow .rounded-10 } 
_Tabnabbing 공격 절차_

1. 사용자가 현재 합법적인 브라우저(A) 탭에서 새 탭으로 여는 웹사이트(실제로는 피싱 사이트)(B)를 클릭한다.
    - (B) 사이트에는 `window.referrer`와 `window.opener` 속성이 존재한다.
    - 이때, 자바스크립트를 사용해 `opener`의 `location`을 피싱 목적으로 원래의 (A) 페이지의 /login 페이지로 변경한다.
2. 악성 웹사이트(B)는 '합법적인 웹사이트를 가장한 Fake Website'(C)를 <span class="bg-yllw"><u>사용자에게 가짜 버전으로 강제로 리다이렉션</u></span> 한다.
    - <span class="bg-yllw">**사용자는 로그인이 풀렸다고 생각하고 다시 ID와 PW를 입력한다.**</span>
3. Fake Site(C)에서는 사용자가 입력한 계정 <span class="bg-yllw"><u>정보를 탈취한 뒤 원래의 합법적인(A) 웹 사이트로 리다이렉트</u></span>합니다.
    - 여기서 A 사이트에 로그인 계정 정보가 그대로 남아있으므로 C 사이트에서 입력해서 A의 홈으로 이동시켰을 때 **자연스러운 전환이기 때문에 이상한 낌새를 찾기 어려울 것입니다.**

<!-- <details markdown="1">
<summary> <b class="bg-gray">간단한 예시로 설명하는 Tabnabbing 공격 절차</b></summary>

![img02](/assets/img/posts/front/20231130_06_02.svg){: .w-100 .h-100 .shadow .rounded-10 } 
_Tabnabbing 공격 절차_

1. 사용자가 `cg**m**.example.com`에 접속한다.
2. 해당 사이트에서 `happy.example.com`으로 갈 수 있는 외부 링크를 클릭한다.
3. 새 탭에 `happy.example.com`가 열린다.
    - `happy.example.com`에는 `window.opener` 속성이 존재한다.
    - 자바스크립트를 사용해 `opener`의 `location`을 피싱 목적의 `cg**n**.example.com/login` 으로 변경한다.
4. 사용자는 다시 본래의 탭으로 돌아온다.
5. 로그인이 풀렸다고 착각하고 아이디와 비밀번호를 입력한다.
    - `cg**n**.example.com`은 사용자가 입력한 계정 정보를 탈취한 후 다시 본래의 사이트로 리다이렉트한다.

</details> -->

## **Tabnabbing 상세 예시**

공격자가 소셜 네트워크에 다음 링크를 게시했다고 가정해 보자.

```html
<!DOCTYPE HTML>
<html>
    <body>
        <h1>Crazy deal on sunglasses!!! Limited supplies!!!</h1>
        <a href="https://evilsite.com" target="_blank">CLICK HERE!</a>
    <body>
<html>
```

사용자가 위 링크를 클릭하면 브라우저의 새 탭에 `evilsite.com`이 열린다. 
`target=“_blank”`을 통해 새로운 탭이 열렸기 때문에, <span class="bg-yllw">공격자는 `window.referrer`및 `window.opener`속성을 악용할 수 있다.</span>

`evilsite.com`의 코드가 어떤 모습인지 살펴보자.

```html
<!DOCTYPE HTML>
<html>
    <body>
        <script>
        if (window.opener) {
        window.opener.location="https://fakelogin.com";
        }
        </script>
        <h1>AMAZING DEAL ON STUFF YOU LIKE!!!</h1>
        ...
    <body>
<html>
```

사용자가 `evilsite.com`에서 피싱 사이트를 보고 있는 동안 `evilsite.com`은 사용자의 원래 탭을 피해자가 열었던 웹사이트에서 나온 것처럼 보이는 <u>가짜 로그인 페이지로 리디렉션</u>한다. 예를 들어 사용자가 Facebook에 로그인되어 있으면, 공격자는 Facebook의 실제 로그인 페이지처럼 보이도록 가짜 로그인 페이지를 보여준다.

`window.opener.location`은 리디렉션을 활성화하는 속성이다. 사용자가 리디렉션된 페이지에 자신의 계정 정보를 입력하면 자신도 모르게 해당 정보를 공격자에게 넘겨주게 된다.


## **Tabnabbing 대응책**

### **1. HTML 제어**

HTML에서는 rel HTML Attribute를 웹 서버/애플리케이션이 <span class="bg-yllw">**링크를 만들때 마다 `noreferrer` 및 `nooperner` 매개변수를 사용하여 설정해줘야 한다.**</span>

위의 설정하는 예시는 다음과 같다.

```html
<!DOCTYPE HTML>
<html>
    <body>
        <h1>Crazy deal on sunglasses!!! Limited supplies!!!</h1>
        <a href=“https://evilsite.com" rel=“noopener noreferrer" target="_blank">CLICK HERE!</a>
    <body>
<html>
```

- `noopener`은 소스 페이지로부터 **`window.opener`에 접근할 수 없도록 한다.**
- `noreferrer`은 HTTP 요청을 보낼 때 referer header가 전송되지 않는지 확인한다. 즉, 하이퍼링크로 이동할 때 **referer header를 생략하고 참조자 정보를 누출하지 않게 막는다.**

`noopener`은 [Window Opener Demo](https://labs.jxck.io/noopener/) 페이지를 통해 테스트해볼 수 있다. 크롬은 버전 49, 파이어폭스 52부터 지원한다. <span class="ft-small">(*자세한 지원 여부는 [Link types](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel)를 참고.*)</span>


### **2. JavaScript 제어**

JavaScript를 사용하면, **속성을 null로 설정**하여 위와 동일한 결과를 얻을 수 있다. 

```javascript
var newWindow = window.open()

newWindow.opener = null
```

사용자가 만든 콘텐츠를 보여줄 경우 서버가 사용자 입력을 검사하고 생성된 각 링크에 `nooperner`, `noreferrer`를 적용해야 한다.

```javascript
var newWindow = window.open(url, name, 'noopener,noreferrer')

newWindow.opener = null
```

### **3. 라이브러리 사용**

이러한 공격이 우려스러운 서비스라면 `blankshield` 등의 라이브러리를 사용한다.

```javascript
blankshield(document.querySelectorAll('a[target=_blank]'));
```

<br/>

> **추가적인 이점**

`noopener` 속성은 이 외에도 성능 상의 이점도 있다. `_blank` 속성으로 열린 탭(페이지)는 언제든지 `opener`를 참조할 수 있다. 그래서 <u>부모 탭과 같은 스레드에서 페이지가 동작</u>한다. 이때 새 탭의 페이지가 리소스를 많이 사용한다면 덩달아 부모 탭도 함께 느려진다. `noopener` 속성을 사용해 열린 탭은 부모를 호출할 일이 없어진다. 따라서 **같은 스레드일 필요가 없으며 새로운 페이지가 느리다고 부모 탭까지 느려질 일도 없다.**
<br/>
<span class="ft-small">*성능 상의 이점에 대한 자세한 내용은 [The performance benefits of rel=noopener](https://jakearchibald.com/2016/performance-benefits-of-rel-noopener/)을 참고.*</span>

<br/>

🔗 **ref :** 
: - [Tabnabbing 공격과 rel=noopener 속성](https://blog.coderifleman.com/2017/05/30/tabnabbing_attack_and_noopener/)
- [Tabnabbing 기법을 통한 계정 탈취](https://www.igloo.co.kr/security-information/tabnabbing-%EA%B8%B0%EB%B2%95%EC%9D%84-%ED%86%B5%ED%95%9C-%EA%B3%84%EC%A0%95-%ED%83%88%EC%B7%A8/)
- [リンクのへの rel=noopener 付与による Tabnabbing 対策](https://blog.jxck.io/entries/2016-06-12/noopener.html)
- [The performance benefits of rel=noopener](https://jakearchibald.com/2016/performance-benefits-of-rel-noopener/)
- [What is reverse tabnabbing and how can you prevent it?](https://www.comparitech.com/blog/information-security/reverse-tabnabbing/)