---
published: true
title: "BrowserRendering - 2 - 브라우저 렌더링과 렌더링 엔진"
categories:
  - FrontEnd
tags:
  - FrontEnd
---

## 브라우저의 기본 구조

![image](https://user-images.githubusercontent.com/60723373/172888008-1bf37b90-f64b-4862-9b27-07c8792b0ce7.png)

1. **사용자 인터페이스** - 주소 표시줄, 이전/다음/새로고침 버튼 등 웹 페이지를 제외하고 사용자와 상호작용합니다.

2. **브라우저 엔진** - 사용자 인터페이스와 렌더링 엔진을 연결하고 둘 사이의 동작을 제어합니다.

3. **렌더링 엔진** - HTML과 CSS를 parsing하여 요청한 웹 페이지를 표시해줍니다.

4. **통신** - HTTP 요청과 같은 네트워크 호출에 사용됩니다. 이 것은 플랫폼 독립적인 인터페이스이고 각 플랫폼 하부에서 실행됩니다.

5. **UI 백엔드** - 체크박스나 버튼같은 기본적인 위젯을 그려줍니다. 플랫폼에서 명시하지 않은 일반적인 인터페이스로서, OS 사용자 인터페이스 체계를 사용합니다.

6. **자바스크립트 해석기(인터프리터)** - 자바스크립트 코드를 해석하고 실행합니다. => chrome은 v8임..

7. **자료 저장소** - localStorage나 Cookie와 같이 보조 기억장치에 데이터를 저장하는 부분입니다.

브라우저의 기본구조는 위와 같이 7개의 요소로 이뤄집니다.

여기서 더 알아야 할 부분이 있는데...

- 자바스크립트 해석기와 렌더링 엔진은 브라우저 별로 다릅니다.
- 각 브라우저 별로 브라우저 구조가 다릅니다.

### 각 브라우저 별 자바스크립트 해석기

1. Rhino - 모질라
2. SpiderMonkey - 파이어폭스
3. V8 - 구글, 오페라
4. JavascriptCore - 사파리
5. Chakra - 익스플로러, 마이크로소프트 엣지

### 각 브라우저 별 렌더링 엔진

1. Gecko - 모질라, 파이어폭스
2. Blink - 구글, 오페라
3. Webkit - 사파리
4. Trident - 익스플로러
5. EdgeHTML - 마이크로소프트 엣지

### 파이어폭스 브라우저 구조의 모습

![image](https://user-images.githubusercontent.com/60723373/172893452-61f4c497-b48c-4ff5-b48c-a783271d83f8.png)

### 크롬 브라우저 구조의 모습

![image](https://user-images.githubusercontent.com/60723373/172893522-e62610d1-0498-423a-ad8e-c7292927270a.png)

## 브라우저 렌더링 동작 과정

브라우저 렌더링의 동작과정을 간단하게 알아봅시다.

1.  사용자가 주소표시줄에 URI를 입력하여 브라우저 엔진에 전달합니다.
    => URL과 URI의 차이가 뭐죠(주소 예정)

2.  브라우저 엔진은 주소를 이용해서 HTML, CSS, image등을 가져옵니다.
    => [주소창에 구글을 치면 어떻게 될까?](https://suhwan2004.github.io/frontend/BrowserRendering1/)

3.  렌더링 엔진은 브라우저 엔진에서 가져온 자료들을 분석합니다. 동시에 URI 데이터를 통신, 자바스크립트 해석기, UI 백엔드로 전파한다.

4.  또한 렌더링 엔진은 통신 레이어에 URI에 대한 추가 데이터(있다면)를 요청하고 응답할 때까지 기다립니다.

5.  응답받은 데이터에서 HTML, CSS는 렌더링 엔진이 파싱합니다.

6.  응답받은 데이터에서 JavaScript는 JavaScript 해석기가 파싱합니다.

7.  JavaScript 해석기는 파싱한 결과를 렌더링 엔진에 전달하여 3번과 5번에서 파싱한 HTML의 결과인 DOM tree을 조작합니다.

8.  조작이 완료된 DOM node(DOM tree 구성요소)는 render object(render tree 구성요소)로 변합니다.

9.  UI 벡엔드는 render object를 브라우저 렌더링 화면에 띄워줍니다.

## 렌더링 엔진의 동작 과정

![image](https://user-images.githubusercontent.com/60723373/172893881-678a03b1-394f-46cb-9ab1-5d1ec5b91c26.png)

랜더링 과정에 대한 사진은 위와 같습니다.

![brouser3](https://d2.naver.com/content/images/2015/06/helloworld-59361-3.png)

또한, 위의 사진은 Webkit의 동작과정의 모습입니다. 각 브라우저별로 단어의 차이는 있으나 전체적인 순서는 비슷합니다.

아래부터, 각 과정에 대해 설명해보겠습니다.

### 1. HTML을 parsing하여 DOM Tree를 생성한다.

DOM은 마크업과 1:1 관계를 맺습니다. 예를 들면 이런 마크업이 있다고 해볼께요.

```html
<html>
  <body>
    <p>Hello World</p>
    <div><img src="example.png" /></div>
  </body>
</html>
```

위의 마크업은 아래와 같은 DOM Tree로 변환할 수 있습니다.
![image](https://user-images.githubusercontent.com/60723373/172902952-ca6c4886-9bc5-475c-9089-cf88463af567.png)

이런 DOM Tree로 만들기 위해서는 아래와 같은 파싱 알고리즘을 거쳐야 합니다.
![image](https://user-images.githubusercontent.com/60723373/172903458-0ce1e00e-f0df-47ce-aa39-6f6e905d3c14.png)

이렇게 HTML 문서를 파싱하고, 파싱 트리를 생성합니다.

여기서 문서 파싱은 브라우저가 코드를 이해하고 사용할 수 있는 구조로 변환하는 것을 의미합니다.

파싱 트리를 기반으로 DOM 요소와 속성 노드를 가지는 DOM 트리를 생성합니다.

### 2. CSS(style sheets)를 parsing하여 스타일 규칙을 얻는다

![image](https://user-images.githubusercontent.com/60723373/172904762-c027568f-d67c-43e0-8320-9118f86e8c5b.png)
위 사진은 웹킷의 css 파싱 과정입니다.

HTML 파싱과 비슷한 느낌으로 CSS또한 파싱해줍니다.
=> 그 결과로 CSSOM이 생성됩니다.

### 3. DOM Tree를 생성하는 동시에, 이미 생성된 DOM Tree와 스타일 규칙(CSSOM)을 Attachment한다

DOM Tree가 구축이 되어가는 동안 브라우저는 DOM Tree를 기반으로 렌더 트리를 생성해줍니다.

문서를 시각적인 구성 요소로 만들어 주는 역활을 해준다고 볼 수 있습니다.

여기서 알고 있어야 되는 점은

- **렌더 트리는 DOM 노드 하나, 하나가 생성될 때 마다 렌더 오브젝트가 추가**되며 이 것들이 렌더 트리가 됩니다.

- **모든 DOM 노드가 렌더 오브젝트가 되는건 아닙니다.**(head tag, display none tag...)

- **html, body tag DOM 노드** 또한 렌더 오브젝트로 구성되긴 하는데 얘네들은 **render tree root로써 render view라고 부릅니다.**

- **렌더 오브젝트는 render tree root에 추가된다**는 사실!

- 렌더 트리는 위치와 크기를 가지고 있지 않기 때문에, 렌더 오브젝트들에게 **위치와 크기를 결정**해준다.

### 4. 구축한 render tree를 배치(layout)한다

렌더링 트리는 위치와 크기를 가지고 있지 않기 때문에, 렌더 오브젝트들에게 위치와 크기를 결정해줍니다.

### 5. 배치가 끝난 render tree를 그린다.

렌더 트리의 각 노드를 화면의 픽셀로 나타내집니다.

렌더 트리 그리기가 완료되면, 화면에 콘텐츠가 표현됩니다.

## 마무리

브라우저 렌더링과 렌더링 엔진의 동작 과정에 대해 간단하게 알아 보았습니다.

결론적으로 렌더 트리가 만들어져 화면에 콘텐츠가 나온다고 해도 다시 이 작업들을 해야되는 상황이 존재합니다.

다음 글에서는 이런 상황에서 최적화를 어떤 부분에서 잡아낼 수 있는지 알아보겠습니다.

## Reference

- https://davidhwang.netlify.app/Developments/browser-rendering-process/
- https://d2.naver.com/helloworld/59361
- https://all-young.tistory.com/22
- https://www.youtube.com/watch?v=EBe-OHkf9w8
- https://www.youtube.com/watch?v=sJ14cWjrNis
