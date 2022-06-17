---
published: true
title: "BrowserRendering - 3 - 브라우저 렌더링 최적화"
categories:
  - FrontEnd
tags:
  - FrontEnd
---

## 렌더링이 다 끝나면 장땡 아닌가요?

아닙니다. 분명, 저희가 전 시간에 일련의 과정을 통해 브라우저 렌더링이 어떻게 되는지 이해했습니다.

그러나, 화면이 떴다고 해서 전부가 아니라 예를들어 사용자의 버튼 클릭에 다음 페이지를 보여준다던가 하는 상황에서는 분명 그에 영향을 받는 자식, 부모 노드들을 포함하여 Layout(Reflow)과정을 다시 수행해야 합니다.

여기서 두 개의 과정이 나옵니다.

## Reflow

Reflow란 렌더 트리와 각 요소들의 크기와 위치를 다시 계산하게 되는 과정입니다.

구체적으로 Reflow가 일어나는 상황은 뭘까요?
다음과 같습니다.

1. 페이지 초기 렌더링 시(최초 Layout 과정)
2. 브라우저 리사이징 시 (Viewport 크기 변경)
3. 노드 추가 또는 제거
4. 요소의 위치, 크기 변경
5. 폰트 변경과 이미지 크기 변경

## Repaint

Repaint란 Reflow된 렌더 트리를 다시 화면에 그려주는 과정입니다. Repaint가 일어나지 않으면 Reflow에서 바뀐 렌더 트리가 저희 눈에 보이지 않습니다.

이렇게, 보면 Reflow -> Repaint인 과정은 맞긴 한데 무조건 Reflow가 발생해야 Repaint가 수행되는 것은 아닙니다.

background-color, opacity와 같이 **레이아웃의 수치를 변화시키지 않는 스타일의 변경이 일어났을 때는 Reflow를 수행할 필요가 없기 때문에 Repaint만 수행하게 됩니다.**

이 두 개의 과정은 시간을 많이 잡아먹기 때문에 최적화가 필요합니다.

## 그럼 최적화는 어떻게 하나요?

사용자의 특정 행동에 따라 저희는 Reflow -> Repaint라는 과정을 통해 화면이 다시 렌더링 됨을 알았습니다.

그럼, 이 과정을 최적화 하는 방법이 뭐가 있을까요?

### 1. Reflow가 일어나는 속성 지양하기

Reflow를 줄이면 당연히 Repaint도 줄어듭니다. 방금 위에 있던 얘기중에 **레이아웃의 수치를 변화시키지 않는 스타일의 변경은 Repaint만 수행**한다고 했습니다.

그렇다면 실제 CSS를 짤 때 Reflow가 일어나는 속성을 지양하면 성능 개선이 가능하겠죠?

**Reflow가 일어나는 대표적인 속성**

```
position, width, height, left, top, right, bottom, margin, padding, border, border-width,
clear, display, float, font-family, font-size, font-weight, line-height, min-height,
overflow, text-align, vertical-align, white-space...
```

**Repaint가 일어나는 대표적인 속성**

```
background, background-image, background-position, background-repeat, background-size,
border-radius, border-style, box-shadow, color, line-style, outline, outline-color,
outline-style, outline-width, text-decoration, visibilty...
```

### 2. 영향을 주는 노드 최소화하기

Js와 CSS를 조합해 애니메이션이나 레이아웃의 변화가 많은 요소의 경우 position을 absolute, fixed를 사용하면 영향을 받는 노드들을 줄일 수 있습니다.

### 3. 프레임 줄이기

단순하게, 0.1초 -> 1px씩 이동하는 걸 0.3초 -> 3px씩 이동한다면 Reflow 비용이 3배 줄은 거라고 볼 수 있습니다. 이렇게 프레임을 줄이면 성능이 향상됩니다.
![image](https://user-images.githubusercontent.com/60723373/174309800-55a8e64f-09c5-45e0-a82c-7c7755a1b269.png)

(시각을 포기한 극한의 성능충이 아닐까...)

### 4. 블록 리소스 최적화

우선, CSS와 JS는 블록 리소스입니다. HTML parsing 중 CSS와 JS를 만나면 parsing이 중단되는 현상이 발생하는데 이런 현상을 발생시키는 리소스를 블록리소스라고 합니다.

그렇다보니, CSS와 JS 둘 다 넣어야 되는 위치가 참 중요합니다. 먼저 CSS부터 보겠습니다.

랜더 트리를 구성하기 위해서는 DOM 트리와 CSSOM 트리가 필요합니다. DOM 트리는 태그를 발견할 때마다 순차적 구성이 가능하지만, **CSSOM 트리
는 CSS를 모두 해석해야 구성할 수 있습니다.**

CSSOM 트리가 완성되지 않으면 렌더링이 차단되기 때문에 이러한 이유로, CSS를 **'렌더링 차단 리소스'** 라고 하고 **렌더링이 차단되지 않도록 CSS는 가능하면 head 태그 안쪽에 배치해야 합니다.**

특정한 경우에서만 사용하는 CSS를 불러올 때는 미디어쿼리를 사용하여 CSS를 분리 후, 필요할 때만 로드될 수 있도록 하는 것이 좋습니다.

다음은 JS입니다.

JS의 경우, body 태그 중간에서 불러올 시 지금까지 파싱된 태그에만 접근이 가능하기 때문에...

1. 순수 script만 쓴다고 한다면 body 태그 맨 마지막에 넣는 것이 맞고
2. 그러기 싫다면 defer 속성을 추가하여 head 태그 안에 넣으면 됩니다.

[script 태그와 관련된 글은 요기 있으니 한번 읽어보셔도 좋을 것 같습니다.](https://suhwan2004.github.io/frontend/ScriptdeferVSasync/)

이외에도 Lazy loading 구현으로 리소스 양 줄이기, 트리 쉐이킹 구현등이 있으나 이는 본질적인 구현부분에 가까워 넣지 못했습니다.

추후에, Lazy loading 관련하여 실제로 만들어보고 이 곳에 링크를 추가할 예정입니다.

## 회고

자세히 알아보기 전에는 솔직히 브라우저 렌더링에 관해서는 "그냥 서버에서 파일들 받아와서 렌더링 하는거 아니야? "라고 생각했었습니다.

![image](https://user-images.githubusercontent.com/60723373/174312887-d3ddebb9-7e0d-4130-9d67-a41e701a9709.png)

(더닝 크루거 효과 곡선. 요즘들어 뼈저리게 느끼고 있습니다.)

이번에 정리가 허술한 것들도 참 많았지만 적으면서 나 정말 구멍 술술 뚫려있구나... 이런 생각이 들던 것 같습니다ㅋㅋㅋ...

우매함의 봉우리에서 내려와서 꾸준히 달려야겠습니다.
