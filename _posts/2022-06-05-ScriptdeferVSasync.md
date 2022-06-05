---
title: "script defer vs async"
categories:
  - FrontEnd
tags:
  - HTML
	- JavaScript
---

![가슴이 웅장해진다(deferVSasync)](https://user-images.githubusercontent.com/60723373/172036374-f90bcf26-1ec9-4c97-9f7e-3f4f8b544573.jpg)
script 태그 내에 세계관 최강자인 defer과 async의 모습이다. 가슴이 웅장해진다...
ㅤ

## script 태그는 뭘까?ㅤ

HTML과 javascript는 땔레야 땔 수 없는 관계이다. HTML의 경우 정적인 콘텐츠 제공이 한계였고, javascript라는 언어가 생기고 웹에 적용됨에 따라 웹에 동적인 콘텐츠가 제공되면서 현재 우리가 보는 웹이 생겼다.

script 태그는 이러한 동적인 웹을 만들기 위해, javascript 파일을 받아오고 parsing해서 실행하기 위한 html 태그이다.

태그 내부에 코드를 직접 삽입하는 경우도 있지만, 보통은 **src="파일경로"**
를 넣어서 javascript 파일을 받아오는 경우가 많다.

자, 우리가 이러한 html 파일을 만들었다고 가정해보자.

```
index.html
<html>
	<head>
		<script src="./src/index.js"></script>
	</head>
	<body>
		<div class="App"></div>
	</body>
</html>
```

```
index.js


let $App = document.querySelector('.App');

$App.innerText = "테스트용 문장입니다."
```

index.js를 보면

1. document.querySelector(".App")을 사용해서 우리가 body 태그에 만들었던 div 태그를 찾는다.
2. 찾은 div 태그내에 "테스트용 문장입니다."를 넣어준다.

우리의 가정이 맞다면 index.js를 받아와서 div 태그내에 문장을 넣어주고, 우리는 html 파일을 화면에 띄웠을 시에 그 문장이 화면에 보여야 된다.

하지만, 아쉽게도 결과는 다음과 같았다.

![script오류](https://user-images.githubusercontent.com/60723373/172036827-c72b8c62-5e01-418e-a5e8-0651e70472b6.png)

개발자 도구 내에 console가 건네는 말을 대충 해석해보면 대충 이렇다.
=> **"나 지금 innerText를 할려 하는데 너가 말한 태그 없는데?"**

왜 이런 문제가 발생하는걸까? 이 문제가 발생하는 이유는 script 태그가 어떻게 동작하는지 보면 알 수 있다.

html 파일을 읽을 때는 보통 위에서 아래로 내려가면서 읽는다.

자 그럼, 우리는 html과 head 태그를 본 뒤 script 태그를 마주칠텐데 **script 태그는 하던걸 다 멈추고 현재까지 html 파일을 보면서 만든 DOM을 기반으로 javascript 파일을 실행한다.**
(DOM은 지금까지 본 HTML 태그를 저장한 곳이라고 이해하면 편하다.)

그럼 과정은 대충 이렇다.

1. script 태그를 만나 index.js를 실행한다.
2. div 태그를 확인하며 DOM에 저장한다.

한 마디로, 지금 상황은 아직 div 태그를 보지도 않았는데 없는 태그를 찾아서 innerText를 할려고 했기에 발생한 문제라는 것이다.

그럼, 이 문제를 당장 해결하려면 어떻게 해야할까?
=> 간단하다. 그냥 script 태그를 html 태그들을 다 만든 뒤에 넣으면 된다.

```
index.html
<html>
	<head></head>
	<body>
		<div class="App"></div>
		<script src="./src/index.js"></script>
	</body>
</html>
```

![결과창](https://user-images.githubusercontent.com/60723373/172037620-3f61c3fb-223b-42ae-9bba-d842a9568c98.png)
=> 이렇게 정상적으로 동작이 되는 모습을 볼 수 있다.

ㅤ
ㅤ

## defer vs async

일단, 우리는 위에서 body 태그 맨 뒤에 script 태그를 넣음으로써 문제를 어느정도 해결했다.

하지만, **head에는 설정이나 외부에서 받아오는 파일 실행** , **body에는 정적 태그 추가**로 따로따로 분리된 모습을 보여주면 참 깔끔할 것 같은데 뭔가 마음에 들지 않는 구성이다.

이걸 또 해결할 수 있는 방법이 있다. **script 태그 내에 defer 또는 async 속성을 넣어주면 된다.**

### 1. defer

![script_defer_parsing](https://user-images.githubusercontent.com/60723373/172037852-db011e8f-c6b8-4e5c-a1a1-ddb2ce286498.png)
defer 속성은 HTML parsing 진행 시 Script fetch 까지만 진행하고, HTML parsing이 끝날 때 받아왔던 script를 실행시키는 방식이다.

이렇게 되면 DOM을 활용할 수 있을 때에 Script가 실행되므로 우리가 맨 처음 봤던 index.html 파일이 정상작동 가능하다.

```
index.html
<html>
	<head>
		<script defer src="./src/index.js"></script>
	</head>
	<body>
		<div class="App"></div>
	</body>
</html>
```

=> defer 태그 하나만 넣었을 뿐인데 정상 작동이 가능한 모습이다.

ㅤ
ㅤ
ㅤ

### 2. async

![script_async_parsing](https://user-images.githubusercontent.com/60723373/172037971-507c46ba-9784-4fbf-baea-b582289de424.png)
async 속성의 경우 HTML parsing을 진행하면서 Script fetch가 가능한 것 까지는 defer와 같으나...
=> **다 받아온 이후에 하던걸 멈추고 바로 Script를 실행시킨 뒤에 다시 HTML parsing을 한다.**

또한, script를 여러 받아올 때 실행순서가 정해져 있지 않고 **말 그대로 fetch 다 된 선착순으로 실행시킨다.**

ㅤ
ㅤ
ㅤ

### 3. 둘 중에 그럼 뭘 써야되죠?

솔직히, 말하면 필자는 defer을 추천한다. 보통 웹 개발을 위해 짜는 javascript 코드의 경우 DOM과의 연결성이 존재하는 경우가 매우 많다.

그렇다 보니, 여전히 async 태그를 썼을 때에도 파싱이 종료되는 순간 DOM 내부에 원하는 태그가 없다면 맨 첫 경우처럼 오류가 발생할 것이다.

async를 써야 겠다고 한다면

1. DOM과의 연결성이 없는 코드를 쓰고
2. 여러 개의 script를 async 속성을 통해 불러올 때 그 순서가 상관 없을 때

이 두 경우에 쓸 수 있을 것이다.

**결론 : 가능하면 그냥 defer 속성을 쓰자!**
