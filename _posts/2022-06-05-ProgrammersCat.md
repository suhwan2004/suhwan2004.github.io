---
title: "프로그래머스 프론트엔드 과제 - 고양이 사진첩"
categories:
  - FrontEnd
tags:
  - JavaScript
---

## 이건 왜 공부했나요?

지금까지를 돌아보면,

- 어떤 식으로 코드를 구현해야 되는지 구조에 대해 감이 안 잡히던 경우가 많았음.
- 기능만 생각하고 구현에 중심을 두다 보니 코드가 더러워지던 경우가 꽤 있었음.

위의 부분들을 해결하기 위해, 공식 사이트에 올라온 고양이 사진첩의 문제 해설 페이지를 보고 코드를 따라가며 공부했다.

공부함으로써 얻을 수 있었던 것

1. Vanila JavaScript를 통한 SPA 방식 만들기
2. 선언적 프로그래밍, 컴포넌트 추상화
3. 컴포넌트 간의 의존도 줄이기, 상태값을 통한 렌더링 구현
4. 나보다 더 실력좋은 사람은 어떻게 코드를 짰을까?

1번의 경우, 넷플릭스나 다른 유명한 회사들 또한 SPA 방식을 많이 사용한다고 들었고 이에 배워보고 싶은 생각이 들었던 것 같다.

?? : SPA가 뭐가 좋은데 공부한다는 거죠?
=> (링크 참고)

React의 경우 react-router나 Next.js가 존재하기 때문에 어느정도 손 쉽게 SPA 방식을 구현할 수 있으나, Vanila JavaScript로 SPA 방식을 구현하는건 생소한 부분도 있었기 때문에 학습의 목적에 적합했던 것 같다.
ㅤ
ㅤ

2,3,4번은 좀 연계된 부분이긴 한데 지금까지 코드를 짤 때 한 js파일에서 많은 기능을 처리한다던가, 컴포넌트 간의 의존도가 큰 코드를 작성했다던가 하는 것들이 생각보다 꽤 있었다.

확실히, 코드를 오랜만에 봤을 때 이게 뭐지? 이게 어떤 흐름이지? 이런 느낌이 들며 알아보기 힘든 경우가 많았고...

이번 기회에 이런 점들을 개선하기 위해 학습하는 걸 목적으로 잡았다.

아래부터는 프로그래머스 공식 해설에서 나온 해설을 중심으로 작성된 코드를 보며 왜 이런 코드가 나왔으며, 이 파일은 무슨 기능인지를 서술할 예정입니다!

혼자 공부했을 때, 애매하던 부분도 있었는데 Udemy에 비슷한 주제의 내용을 다루는 강의가 존재해서 많은 도움을 받았습니다. 비슷한 방식의 구현 뿐만 아니라...

- 본인이 어떠한 기능을 짜기 전에, 그 기능안에 무엇이 들어가야 되는지 명세하는 것
- 코드를 짠 이후에 가독성과 성능적인 부분을 고려해서 어떻게 리팩토링을 시켜야 할지

등의 내용이 담긴 강의라 치킨 하나 덜 시키고 꼭 한번 들어보는걸 추천합니다.
=> [☕ 블랙커피 Vanilla JS Lv1. 문벅스 카페 메뉴 앱 만들기](https://www.udemy.com/course/vanilla-js-lv1/)

ㅤ

## 과제 구현사항

### 디렉토리 구조를 따라 탐색할 수 있는 사진첩 다이어리

- 디렉토리를 클릭한 경우 해당 디렉토리 하위에 속한 디렉토리 / 파일들을 불러와 렌더링
- 디렉토리 이동에 따라 위에 Breadcrumb 영역도 탐색한 디렉토리 순서에 맞게 업데이트
- root 경로가 아닌 경우 상위 폴더로 돌아갈 수 있는 아이콘
- 파일을 누른 경우 해당 파일의 filePath 값을 통해 이미지를 보여주기
  - 사진 영역 밖을 클릭하거나 esc를 누르면 이미지 닫기

### 구현시 유의 사항

- 각 화면의 UI 요소는 가급적 컴포넌트 형태로 추상화하여 동작해야함  
  각 컴포넌트가 서로 의존성을 지니지 않고, App 또는 그에 준하는 컴포넌트가 조율하는 형태로 동작하는 것을 말함
- ES6 모듈 형태로 작성시 가산점
- api 처리 코드 별도 분리
- 이벤트 바인딩 가급적 최적화

### 필수 구현 사항

- Breadcrumb : 현재 탐색 중인 경로. root를 맨 왼쪽에 넣어야하며, 탐색하는 폴더 순서대로 나타냄
- Nodes : 현재 탐색 중인 경로에 속한 파일 / 디렉토리를 렌더링  
  type: DIRECTORY, FILE, PREV
- ImageView : 파일을 클릭한 경우 Modal을 하나 띄우고 해당 Modal에서 파일의 이미지를 렌더링

### 옵션 구현 사항

- Breadcrumb에 렌더링 된 경로 목록의 특정 아이템을 클릭하면, 해당 경로로 이동하도록 처리
- 파일을 클릭하여 이미지를 보는 경우, 닫을 수 있는 처리를 해야함. ESC키를 눌렀을 때와 이미지 밖을 클릭했을 때, 둘 중 한 가지 혹은 두 가지 모두
- 데이터가 로딩 중인 경우는 로딩 중임을 알리는 UI적 처리를 해야하며, 로딩 중에는 디렉토리 이동이나 파일 클릭 등 액션이 일어나는 것을 막아야 함
- 한번 로딩된 데이터는 메모리에 캐시하고 이미 탐색한 경로를 다시 탐색할 경우 http 요청을 하지 말고 캐시된 데이터를 불러와 렌더링

### API 구조

```
[
  {
  	"id":       string // 문자열로 된 Node의 고유값입니다.
  	"name":     string // 디렉토리 혹은 파일의 이름입니다. 화면에 표시할 때 사용합니다.
  	"type":     string // 파일인지 디렉토리인지 여부입니다. 파일인 경우 FILE, 디렉토리인 경우 DIRECTORY 입니다.
  	"filePath": string // 파일인 경우에 존재하는 값입니다. 해당 파일 이미지를 불러오기 위한 경로가 들어있습니다.
  	"parent":   object | null {
    	"id": string // 해당 Node가 어디에 속하는지 나타내는 값입니다. parent가 null이면 root에 존재하는 파일 / 디렉토리입니다.
  },
}
```

### 폴더 구조

```
│  index.html
│
├─ assets
│  	directory.png
│   file.png
│   nyan-cat.gif
│   prev.png
│   sample_image.jpg
│
└─ src
   │ index.js
   │
   ├─ components
   │    api.js
   │    App.js
   │    Breadcrumb.js
   │    ImageView.js
   │    Nodes.js
   │
   └─ styles
        style.css
```

ㅤ
ㅤ
ㅤ

## 코드 설명

### index.html

```html
<html>
  <head>
    <title>고양이 사진첩!</title>
    <link rel="stylesheet" href="./src/styles/style.css" />
    <script defer src="./src/index.js" type="module"></script>
  </head>
  <body>
    <h1>고양이 사진첩</h1>
    <main class="App"></main>
  </body>
</html>
```

- body 태그 내에 main 태그 내부에 랜더링을 시킨다.
- head 태그 내에 type = "module" 속성을 추가한 script 태그를 이용하여 js 파일을 불러온다.

여기서 나오는 꼭 알아봐야 되는 점이 두 가지 있다.

1. script 태그를 쓰는건 알겠는데 저기 script 태그 내에 defer 태그는 뭐죠?
   => [script async vs defer](https://suhwan2004.github.io/frontend/ScriptdeferVSasync/)
2. script 태그 내에 type = "module" 저거는 왜 쓰는거죠?
   => [모듈을 뭐고, script type = "module"은 왜 쓰는걸까?](https://suhwan2004.github.io/frontend/TypeModule/)

각 질문은 저 링크를 타고가면 해결 가능하다!
ㅤ
ㅤ
ㅤ

### index.js

```js
import App from "./components/App.js";

new App(document.querySelector(".app"));
```

- import문을 실행하여 ./components/App.js 파일을 가져온다.
- App은 App.js 내에 export defalt 가 붙은 함수이다.
- new 생성자를 통해 App을 생성하여 사용한다.

import의 경우 es6부터 생긴 module을 가져와서 사용할 수 있게 해주는 구문이다.

이럴 때의 장점은 각 기능에 따라 js 파일을 나누고, 필요할 때만 import를 통해 불러와서 사용할 수 있기 때문에 재사용성도 올라가고, 좀 더 깔끔한 프로젝트를 만들 수 있다.

ㅤ
ㅤ
그리고, **여기에서 진짜 중요한 게 하나 있는데 저기 세 번째 항목이다.**

### - new 생성자를 통해 App을 생성하여 사용한다.

여기에서 위에 처럼 생성자 방식으로 객체를 만들어서 사용하면 문제가 없지만, **new를 빼고 App()처럼 말 그대로 함수를 불러와 실행키면 얄짤없이 오류가 발생한다.**

![image](https://user-images.githubusercontent.com/60723373/172039451-41510c16-d0fb-4b48-a450-0eaeb6abdc1c.png)

뒤에도 계속 코드를 설명하겠지만, App.js 코드 내에는 function App() 내부에서 this.~~ 같은 방식으로 자신이 현재 속해있는 this에 프로퍼티를 넣고 활용하는 방식으로 진행된다.

그런데...

1. index.js에서 App(~~) 이렇게 함수 실행 방식으로 코드를 짜고
2. App.js 내에 function App(){} 내부에서 this 키워드를 사용하기만 하면 오류가 나는 것이다.

ㅤ
ㅤ
이 문제가 발생하는 이유는 this의 호출 방식에 따른 동적으로 바인딩 되는 특징 때문이다.

1. 먼저, 생성자 함수 방식으로 App을 사용했을 때에 this는 **자신을 생성한(또는 생성할) 인스턴스**에 바인딩 된다.

2. 함수 호출 방식으로 App을 사용했을 때에 this는 **Strict 모드를 사용했을 때 undefined, 사용하지 않았을 때 전역객체(window)** 가 반환된다.

3. **또한, 모듈의 최상위 this도 undefined이다.**

이 프로젝트의 경우 모듈을 import 하는 방식으로 App을 불러왔기 때문에 App()방식을 사용했을 때, App 내에서 this.state이 undefined.state를 한 것과 같아서 저런 오류가 난 것이다.

또한 this는 화살표 함수로 함수를 만들었는지, 일반적으로 함수를 만들었는지에 따라서도 바인딩이 되는 대상이 다른 등 정말 알 수 없는 친구다.

아래는, this에 관해 필자가 정리한 것이다. 여러분은 꼭 보고 예방하길...
=> (this 관련 공부한 것 주소 올릴 예정)

ㅤ
ㅤ

### api.js

```js
const API_END_POINT = "https://~~~~";

export const request = async (nodeId) => {
  try {
    const res = await fetch(`${API_END_POINT}/${nodeId ? nodeId : ""}`);

    if (!res.ok) throw new Error("서버의 상태가 이상합니다!");
    return await res.json();
  } catch (e) {
    throw new Error(`무언가 잘못 되었습니다! ${e.message}`);
  }
};
```

- API_END_POINT에서 원하는 특정 디렉토리들을 받아올 수 있도록 해주는 코드.
- async, await, fetch 사용.
- export를 통한 모듈화 구현

api.js는 원하는 정보를 프로그래머스에서 지정해준 주소와 통신을 통해 받아오기 위해 작성된 코드이다.

여기서 알고가면 참 좋은 것을 정리해보자면

1. request를 화살표 함수로 구현했는데 앞에 보면 **export** 라는 키워드가 있는 것.
   => export는 아까 전에 나왔던 import와 단짝 친구이다. 다른 곳에서 impor 하고 싶은 모듈 내 함수가 존재한다면 export 키워드를 넣어줘야 사용 가능하다.

2. async, await, fetch가 뭐죠?
   => fetch는 원하는 정보를 받기위해 통신할 때 사용되는 함수이다. async, await의 경우, js 코드가 위에서 내려오면서 실행될 때 이 키워들을 만나면 **"꼭 이 함수가 다 작업이 되고 다음으로 넘어가라"** 이 의미이다.

ㅤ
ㅤ
?? : 그냥 fetch만 써도 되는 거 아닌가요?
=> 라고 물어볼 수도 있는데 이게 위에서 봤던 script 대란과 비슷한 상황이 발생할 수 있다.

ㅤ
ㅤ
자 그럼, async, await를 안 썼을 때 발생하는 참사를 확인해보자.

1. 어머니가 된장찌개를 만들어야 된다고 해서 나는 마트에 갔다.
   => (fetch 시작)
2. 두부를 사고 오니 어머니가 이미 두부없는 된장찌개를 다 끓이셨다.
   => (fetch가 끝나기 전에 fetch로 받아올 정보를 활용한 함수 실행)
3. 바로 오지 않아서 두부 없는 된장찌개가 만들어졌다고 화를 내셨다.
   => (정보가 없어서 오류 발생)

ㅤ
ㅤ
결국, 심부름을 갔던 수환이는 기분만 나빠졌다. Javascript 엔진의 경우 **단 하나의 실행 컨텍스트 스택**을 갖기 때문에 어머니와 내가 동시에 일을 처리한다는 선택지는 존재하지 않는다.
=> **실행 컨텍스트 스택, 이벤트 루프, 테스크 큐**를 공부하면 더 깊게 공부할 수 있다. (주소 추가 예정)

한 마디로, 이 문제를 해결하기 위해서는 어머니가 하던 일을 잠시 스탑하고 내가 두부를 사온 뒤에 다시 요리를 계속하는 방법이 유일한 해결책이다. 이걸 Javascript에서는 **비동기 프로그래밍**이라고 한다.
=> 풍문에 듣기로는 [Web Worker API](https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API/Using_web_workers) 사용시, 브라우저에서 멀티 쓰레드처럼 동작이 가능하도록 할 수 있다고 한다.

ㅤ
ㅤ
비동기 프로그래밍을 구현하기 위해서는 총 3가지 선택지가 있다.

1. CallBack function
2. Promise
3. async, await

async, await는 Promise를 좀 더 깔끔하게 짜기 위해 나온 것이라 가능하면 **async, await를 쓰는 것을 추천**한다. CallBack function의 경우 **Callback Hell**이 발생하면 답이 없기 때문에 추천하지 않는다.
=> (셋에 대한 내용을 정리한 주소 첨부 예정)

ㅤ
ㅤ

### App.js

```js
import Breadcrumb from "./Breadcrumb.js";
import Nodes from "./Nodes.js";
import ImageView from "./ImageView.js";
import Loading from "./Loading.js";
import { request } from "./api.js";

const cache = {};

export default function App($app){
	this.state  =  {
		isRoot: false,
		nodes: [],
		depth: [],
		selectedFilePath: null,
		isLoading: false,
	};


//이 네 줄을 잘 기억하자. 끝에서 설명예정!
const breadcrumb = new Breadcrumb(코드생략);
const nodes = new Nodes(코드생략);
const imageView = new ImageView(코드생략);
const loading = new Loading(코드 생략);

// 상태 변화시 각 객체의 setState를 실행시켜 각각 렌더링 시켜주는 모습.
this.setState  = (nextState) => {
	this.state = nextState;
	breadcrumb.setState(this.state.depth);
	nodes.setState({
		isRoot: this.state.isRoot,
		nodes: this.state.nodes,
	});
};

const init = async() => {코드생략};

//처음으로 화면에 렌더링 시켜주기.
init();
}
```

코드가 너무 길어서 먼저 넓게 보고 이후에 자세히 보고자 한다.

당장 여기서 확인할 수 있는 것들은 이 정도가 있다.

- $app이 뭐지?
- cache를 만들기 위해 object 타입의 상수를 선언했음.
- this.state, this.setState의 구현
- 구현에 필요한 breadcrumb(위에 파일 경로 표시), nodes(파일 또는 디렉토리들 표시), imageView(파일을 눌렀을 때 고양이 이미지 표시), loading( api request 중에 대기화면 표시 : 아까 위에서 봤던 pre-rendering)가 먼저 생성됨.
- 그 다음에 init 실행

$app은 우리가 index.js에서 new App()을 할 때 매개변수로 넣어줬던 **main태그**이다.

그럼, 아래와 같은 질문이 충분히 나올 수 있다.
?? : 근데 이거 그냥 App.js에서 document.querySelector(".App")으로 찾아서 쓰면 되는거 아님??

=> 이는 문제에서도 강조한 **선언적 프로그래밍과 컴포넌트 추상화를 위해서인데** 이건 BreadCrumb.js에서 설명하겠다.
ㅤ
ㅤ
일단, cache를 보자.

우리는 디렉토리를 돌 때마다 그 디렉토리에 있는 파일과 폴더들을 가져오기 위해 계속 request 함수를 돌릴 것이다. 여기서 **성능의 최적화**를 시킬 수 있는 포인트가 존재한다.

고양이 사진첩을 쓰던 중에 분명 갔던 디렉토리를 돌아갈 때도 있을텐데 그런 부분을 싹 다 무시하고 페이지 이동마다 request를 한다?

물론, 웹은 문제없이 돌아가겠지만 분명 공간을 조금만 더 써서 **request를 한 것들을 저장해두면 뒤로가기를 할 때 바로 저장해둔 곳에서 찾아와서 화면만 그려주면 될텐데** 좀 많이 아쉽다.

그런 부분에 의해 구현한 것이 cache이다. cache가 어떻게 구현되어 있는지는 잠시 후, 볼 수있는 new Node()에 매개변수를 넣는 부분에서 나온다.

ㅤ
ㅤ
다음은, this.state와 this.setState이다.

이게 좀 친숙하신 분들도 있을 것 같다. react에 ~.setState를 모방한 부분이라고도 볼 수 있다.

이 고양이 사진첩의 경우, 사용자가 뒤로 가기를 누른다거나, 사진을 클릭한다거나 등의 **특정 행위를 통해서 다시 렌더링**이 된다. 이런 **렌더링을 발생시킬 수 있는 요소들을 저장해둔 것**이 **this.state**이다.

그럼? this.setState는 무엇일까?

1. 바뀐 state인 nextState를 받아서 this.state의 값을 바꿔준다.
2. 바뀐 this.state로 각 객체들의 setState를 실행시켜 각 객체들마다 리랜더링 시켜준다.

이 두가지 기능을 수행하는 함수라고 볼 수 있다. **이 프로젝트에서 App.js는 직접적인 랜더링을 실행하지 않는다.**

단지, 각 객체들 중 어느 한 곳이라도 상태값의 수정을 발생시켰을 때, 그 정보(nextState)를 가져와서 setState를 실행시켜 모든 객체들을 다시 리랜더링 시켜주는 **메인 타워같은 역활**이라고 볼 수 있다.
ㅤ
ㅤ
?? : 그럼 이렇게 메인 타워처럼 있는 이유가 뭔데요? 그냥 각자의 컴포넌트에서 다른 부분의 상태가 변할때마다 렌더링을 직접할 수 있도록 하면 안되나요?

그렇게 짤려면 각각의 함수에서 모든 기능함수에 대한 객체를 만들어서 자신의 기능이 변할 때 다른 객체의 setState들을 실행시키는 방식으로 구현해야 된다.

한 마디로, **만약, 기능 함수가 하나 더 추가 된다면? 모든 js 파일을 찾아가서 객체를 하나 더 새로 만들어야 된다. 한마다로, 서로의 의존성이 매우 높아진다.**

컴포넌트는 **"프로그래밍에 있어서 재사용이 가능한 각각의 독립된 모듈"을 뜻한다.** 과연 그렇게, 구현했을 때 그 것들을 컴포넌트라고 부를 수 있을까..?
ㅤ
ㅤ

다음은, 각각 생성자 함수 호출방식으로 생성된 객체들이다. 이 것들의 경우 밑에서 설명할 예정이나 넓게 보면, 맨 처음의 화면을 띄워주는 init 생성 전에 앞으로 this.setState 발생 시에 상태변화에 따라 같이 다른 화면을 리렌더링 시키기 위해 생성해준다. 물론, 생성되면서 한번씩 실행도 된다.

각 부분마다 뭐가 들어가는지는 해당 생성자 함수의 코드를 먼저 설명하고 설명하겠다.

ㅤ

마지막으로 init는 최초 웹 최초 실행 시 root directory의 데이터를 불러오고 그에 따라 화면을 전시하기 위해 만들어진 함수이다. 최초로 한번 실행된다.

this.init의 흐름은 지금 바로 볼 법 한데 아래가 그 코드이다.

```js
const init = async () => {
  try {
    this.setState({
      ...this.state,
      isRoot: true,
      isLoading: true,
    });
    const rootNodes = await request();
    this.setState({
      ...this.state,
      isRoot: true,
      nodes: rootNodes,
    });
    cache.rootNodes = rootNodes;
  } catch (e) {
    // 에러처리 하기
  } finally {
    this.setState({
      ...this.state,
      isLoading: false,
    });
  }
};
```

여기서 봐야될 포인트는 이 두 가지 정도가 있다.

- try, catch, finally
- 실행 흐름

try, catch, finally 문의 경우, 간단한데 이 흐름을 하고 실행된다.

1. try에서 특정 작업을 수행
2. 작업 중 에러가 나면 catch 문 실행
3. 에러가 나든 안나든 finally 문을 제일 마지막에 꼭 실행

자 그러면, 자연스럽게 이 init라는 함수가 어떻게 실행되는지 알 수 있다.

1. 먼저, this.setState로 화면을 한번 리렌더링 하는데 isloading : true가 되어 있기 때문에 **화면에는 로딩창을 띄워준다.** (pre-rendering)
2. rootNodes를 서버에서 **request**를 통해 가져온다.
3. 완전히 가져오고 나서야 로딩이 되고 있는 상태에서 **루트 디렉토리의 요소들이 화면에 그려진다.**
4. **cache**에 지금 rootNodes를 저장한다. 이제 rootNode로 돌아와도 다시 request를 하지 않아도 된다.
5. finally 문을 실행해서 loading : false로 바꾸고 **로딩창을 치워준다.** 이제 메인 화면이 제대로 보인다!

이 5개의 과정이 이 고양이 사진첩을 켰을 때 제일 먼저 일어나는 일이다.

(이 과제의 경우, catch문이 실행될 확률이 없기 때문에 일단 미구현 상태이나 추후에 추가 기능구현을 해볼 예정이다...)

ㅤ

ㅤ

### Breadcrumb.js

```js
export default function Breadcrumb({ $app, initialState = [], onClick }) {
  this.state = initialState;
  //nav 태그를 하나 새로 만들고 그 것을 $app에 넣음.
  this.$target = document.createElement("nav");
  this.$target.className = "Breadcrumb";
  $app.appendChild(this.$target);

  this.setState = (nextState) => {
    this.state = nextState;
    this.render();
  };

  this.onClick = onClick;

  //nav에 this.state를 기반으로 랜더링 시킴
  this.render = () => {
    this.$target.innerHTML = `<div class="nav-item">root</div>${this.state
      .map(
        (node, index) =>
          `<div class="nav-item" data-index="${index}">${node.name}</div>`
      )
      .join("")}`;
  };

  //뒤로가기 구현
  this.$target.addEventListener("click", (e) => {
    const $navItem = e.target.closest(".nav-item");
    if ($navItem) {
      const { index } = $navItem.dataset;
      this.onClick(index ? parseInt(index, 10) : null);
    }
  });

  this.render();
}
```

자 이제, Breadcrumb.js 를 설명하기 전에 아까 필자가 여기서 설명한다고 했던 것을 설명 하도록 하겠다.

받아온 매개변수를 자세히 보면 $app이 또 존재한다.
=> 이는 위에서도 말했듯이 **선언적 프로그래밍과 컴포넌트 추상화를 구현**하기 위함이다. 이 말은 즉슨 이런 말이다.

1. DOM을 접근하는 부분을 최소화하고
2. 명령형 프로그래밍보다는 재사용이 가능한 선언적인 프로그래밍 방식으로 구현해야하며
3. 어딘가에 묶여있지 않아야 한다.

이 둘은 서로 맞물리는 부분이 존재하는데...

예를 들어 필자가 $app을 받지 않고 함수 내부에서 **const $app = document.querySelector(".App");** 처럼 특정 태그를 딱 골라서 거기에 랜더링을 시킨다면 어떻게 될까?
=> 간단하다. 그 함수는 말 그대로, **"나는 className이 App인 태그에 경로구현을 시각화해주는 것이 오직 내 목표야"** 가 된다. 재사용이 불가하다.

반대로, $app을 매개변수로 넣어준다면?
=> Breadcrumb.js는 **내가 준 태그가 main 태그인지, div 태그인지 중요하지 않다. 말 그대로, "나는 경로 구현을 시각화 해주고 끝이야!" 만 실행해주면 되는 것이다.**

이렇게, 어떤 태그가 존재하던 안하던 Breadcrumb.js는 받은 $app이라는 미상의 태그에 구현만 해주면 된다. 그렇기에 이걸 **컴포넌트**라고 부를 수 있다.

ㅤ

그 이외에 더 봐야 될 포인트는 다음과 같다.

- this.render()
- onClick을 왜 여기서 안 만들고 매개변수로 받아왔죠??

먼저 this.render()을 보자. 현재, 코드를 보면 this.render는 별 다른 매개변수를 요구하지 않는다.
=> 오직 **"this.state"** 를 기반으로 랜더링을 시키는 것을 알 수 있다. 당연히도 현재 상태를 기반으로 랜더링을 시키는 우리의 취지에 맞게 짜져 있다.

ㅤ

다음으로는 onClick event의 동작함수를 왜 App.js에 구현했는지이다. 내가 이해한 것으로 이렇다.

- 각 컴포넌트의 역활은 상태값을 통한 리랜더링만을 오로지 목적으로 두고 만들어졌다.
- 그렇기 때문에, 컴포넌트 내부에서 복잡한 함수를 구현하는 것은 지양해야된다.
- 그저 상위의 모듈에서 해당 기능을 구현하고, 이걸 매개변수로 받음으로써 컴포넌트는 onClick이 무슨 기능이든 상관없이 이걸 실행시키는 업무를 하는 것에 중점을 두게한다.

나머지 기능함수들은 주석에 담긴 내용으로도 충분히 이해 가능하다.

ㅤ

ㅤ

### Nodes.js

```js
export default function Nodes({ $app, initialState, onClick, onBackClick }) {
  this.state = initialState;
  this.$target = document.createElement("ul");
  $app.appendChild(this.$target);

  this.setState = (nextState) => {
    this.state = nextState;
    this.render();
  };

  this.onClick = onClick;
  this.onBackClick = onBackClick;

  //render를 시켜주는 함수.
  //ul tag인 $target에 랜더링 할 태그들을 넣어주는 모습이다.
  this.render = () => {
    if (this.state.nodes) {
      const nodesTemplate = this.state.nodes
        .map((node) => {
          const iconPath =
            node.type === "FILE"
              ? "./assets/file.png"
              : "./assets/directory.png";

          return `<div class="Node" data-node-id="${node.id}">
				<img src="${iconPath}" />
				<div>${node.name}</div>
			</div>`;
        })
        .join("");

      //현재 위치가 루트가 아니라면 뒤로가기 버튼을 먼저 넣어준다.
      this.$target.innerHTML = !this.state.isRoot
        ? `<div class="Node"><img src="./assets/prev.png"></div>${nodesTemplate}`
        : nodesTemplate;
    }
  };

  this.render();

  /*
뒤로가기, 다음 페이지로 이동 구현
*/
  this.$target.addEventListener("click", (e) => {
    const $node = e.target.closest(".Node");
    if ($node) {
      const { nodeId } = $node.dataset;
      if (!nodeId) {
        this.onBackClick();
        return;
      }
      const selectedNode = this.state.nodes.find((node) => node.id === nodeId);

      if (selectedNode) {
        this.onClick(selectedNode);
      }
    }
  });
}
```

지금부터 볼 기능함수들의 구조는 다 비슷하기 때문에 주석을 참고하며, 코드를 한번 훑어보는 방식으로 진행하면 별 문제없이 이해할 수 있을 것이다.

Nodes.js에서 주의깊게 봐야 될 것은 **이벤트 위임(Event Delegation)** 이다.

코드를 보면 **ul 태그**에 클릭 이벤트를 감지했다면, 현재 마우스를 누른 곳에서 가장 가까운 **Node**를 찾아서 이벤트 함수를 실행시킨다.

=> 이렇게 **부모에서 받은 이벤트를 자식에게 전파하는 것**을 **"이벤트 위임"** 이라고 한다.

ㅤ

ㅤ

### ImageView.js

```js
// 이미지를 받아오기 위한 주소
const IMAGE_PATH_PREFIX = "https://~~~";

export default function ImageView({ $app, initialState, modalClose }) {
  this.state = initialState;
  this.$target = document.createElement("div");
  this.$target.className = "Modal ImageView";
  $app.appendChild(this.$target);

  this.setState = (nextState) => {
    this.state = nextState;
    this.render();
  };

  this.modalClose = modalClose;

  //두 개의 이벤트 리스너를 하나로 묶어둔 모습이다.
  this.addModalCloseEvent = () => {
    //content를 클릭 시 Modal 창이 닫히는 이벤트
    this.$target.addEventListener("click", (e) => {
      const $node = e.target.closest(".content");
      if (!$node) {
        this.modalClose();
      }
    });
    //ESC키를 누르면 Modal 창이 닫히는 이벤트
    window.addEventListener("keydown", (e) => {
      if (e.key === "Escape") {
        this.modalClose();
      }
    });
  };

  this.render = () => {
    this.$target.innerHTML = `<div class="content">${
      this.state ? `<img src="${IMAGE_PATH_PREFIX}${this.state}">` : ""
    }</div>`;

    this.$target.style.display = this.state ? "block" : "none";
  };

  this.addModalCloseEvent();
  this.render();
}
```

ImageView.js의 경우, 다른 컴포넌트와 유사한 점이 많으나 딱 2가지 정도만 알면 된다.

1. Node들 중 하나를 클릭하면, **App.js의 this.setState -> ImageView.setState() -> this.render()** 가 실행되고 content의 display 값이 block 되면서 화면 최상단에 숨겨져 있던 Modal창이 뜨도록 랜더링 해주는 것.
2. Modal 창을 닫는 이벤트가 2개 있는 것.

ㅤ

ㅤ

### Loading.js

```js
export default function Loading({ $app, initialState }) {
  this.state = initialState;
  this.$target = document.createElement("div");
  this.$target.className = "Loading Modal";
  $app.appendChild(this.$target);

  this.setState = (nextState) => {
    this.state = nextState;
    this.render();
  };

  /*ImageView와 비슷하게 Loading도 Modal 창을 통해 구현된다는 사실을 알 수 있다.*/
  this.render = () => {
    this.$target.innerHTML = `<div class="content"><img src="./assets/nyan-cat.gif"></div>`;

    this.$target.style.display = this.state ? "block" : "none";
  };

  this.render();
}
```

코드를 천천히 보면 충분히 이해할 수 있다. 마찬가지로, 상태의 변화에 따라 Loading 창을 화면에 렌더링 해주는 것이 Loading.js의 역활이다.

ㅤ

ㅤ

### 다시 App.js로 돌아옴.

자, 이제 우리는 이런 것들을 알 수 있었다.

1. 이 웹 서비스가 상태값을 통해 렌더링 된다는 것.
2. 컴포넌트의 경우 이벤트 리스너를 통해 발생하는 이벤트를 App.js에서 매개변수로 받아 단지 실행할 뿐이라는 것.
3. 각 파일 별로 어떤 기능들이 담당하는 것.

아직 우리가 안 본 것이 있는데 바로 **App.js에서 매개변수 형태로 주는 이벤트 함수와 상태값**이다. 이는 앞 전에, 바로 설명시 혼란을 줄 수 있어서 생략해둔 상태였다. 이제 두려운 것은 없다. 침착하게 어떤 코드인지 보도록 하자.
ㅤ

ㅤ

### new Breadcrumb()

```js
const breadcrumb = new Breadcrumb({
  $app,
  initialState: [],
  onClick: (index) => {
    //index가 null이라면 root로 간주한다.
    if (index === null) {
      this.setState({
        ...this.state,
        depth: [],
        isRoot: true,
        nodes: cache.rootNodes,
      });
      return;
    }
    //breadcrumb에서 현재 위치를 누른경우라 무시하는 경우임.
    if (index === this.state.depth.length - 1) {
      return;
    }

    const nextState = { ...this.state };
    const nextDepth = this.state.depth.slice(0, index + 1);

    //경로를 1 추가하고, 만약 캐쉬에 다음으로 보는 경로의 값이 있다면 nodes에 들어간다. 없으면 request 예정.
    this.setState({
      ...nextState,
      depth: nextDepth,
      nodes: cache[nextDepth[nextDepth.length - 1].id],
    });
  },
});
```

코드의 양이 많은 관계로 먼저 breadcrumb 객체에 넘겨주는 값부터 확인해보자.

- $app의 경우 위치를 넘겨주는 것.
- initialState의 경우 this.state에서 넘겨줘야 되는 값을 넣어주는데 딱히 없다.
- onClick의 경우 아까도 말했듯이 App.js에서 breadcrumb내에서 실행될 이벤트 리스너의 실행 함수를 넘겨주는 모습이다.

onClick이 살짝 복잡할 수 있으나, 주석을 따라가면 충분히 이해 가능할 것이라 믿는다.

ㅤ

ㅤ

### new Nodes()

```js
const nodes = new Nodes({
  $app,
  initialState: [],
  onClick: async (node) => {
    try {
      //1. 일단 로딩시킨다
      this.setState({
        ...this.state,
        isLoading: true,
      });
      //2. 만약 지금 클릭한 node가 디렉토리인가?
      if (node.type === "DIRECTORY") {
        //2-1. 그 전에 캐쉬에 저장한 값이 있다면 request 할 것 없이 재사용하면 된다.
        if (cache[node.id]) {
          this.setState({
            ...this.state,
            isRoot: false,
            depth: [...this.state.depth, node],
            nodes: cache[node.id],
            isLoading: false,
          });
        } else {
          /*2-2. 한 번도 가보지 않은 디렉토리라면 request를 한번 해서 내용물을 받아오고
				 this.state를 해주면서 캐쉬에 추가한다.*/
          const nextNodes = await request(node.id);
          this.setState({
            ...this.state,
            isRoot: false,
            depth: [...this.state.depth, node],
            nodes: nextNodes,
            isLoading: false,
          });
          cache[node.id] = nextNodes;
        }
      } else if (node.type === "FILE") {
        /*3. 만약 파일이라면 사진을 띄워줘야 한다.
			 아까, ImageView에 node.filePath만 넘겨주면 ImageView에 있는
			 엔드포인트와 결합하여 이미지를 받을 수 있다.*/
        this.setState({
          ...this.state,
          isRoot: false,
          selectedFilePath: node.filePath,
          isLoading: false,
        });
      }
    } catch (e) {
      // 에러처리하기
    }
  },
  //뒤로가기를 눌렀을 때 발동되는 이벤트
  onBackClick: async () => {
    try {
      const nextState = { ...this.state };
      nextState.depth.pop();

      //1. nextState.depth를 통해 전 경로를 받아올 수 있다.
      const prevNodeId =
        nextState.depth.length === 0
          ? null
          : nextState.depth[nextState.depth.length - 1].id;
      this.setState({
        ...nextState,
      });
      //2-1. prevNodeId가 없다? 그럼 Root지!
      if (prevNodeId === null) {
        this.setState({
          ...nextState,
          isRoot: true,
          nodes: cache.rootNodes,
        });
      } else {
        //2-2. prevNodeId가 존재한다? 그럼 루트는 아님.
        //전에 갔던 경로는 캐쉬에 무조건 존재한다. 꼭 재사용하자.
        this.setState({
          ...nextState,
          isRoot: false,
          nodes: cache[prevNodeId],
        });
      }
    } catch (e) {
      // 에러처리하기
    }
  },
});
```

살짝 코드가 긴 느낌이 없잖아 있다. 하지만, 천천히 어떤 것들이 매개변수로 들어가는지 분석한다면 문제없이 이해 가능하다.

- $app, initialState는 굳이 설명하지 않겠다.
- onClick : 여기서도 onClick을 App.js에서 구현해주고 단지 넘겨주기만 하는 모습을 보여준다. 여러 개의 조건문을 통해 함수가 동작하는데 이건 주석에 흐름을 충분히 이해가 되도록 작성하였다.
- onBackClick : 마찬가지로 App.js에서 구현했다. 이 또한, 흐름에 따라 주석을 작성해두었다.

코드만 많지, 안에 구현되어 있는 내용은 차분히 읽어보면 충분히 이해가 가는 내용이다.

ㅤ

### new ImageView(), new Loading()

```js
const imageView = new ImageView({
  $app,
  initialState: this.state.selectedFilePath,
  modalClose: () => {
    this.setState({
      ...this.state,
      selectedFilePath: null,
    });
  },
});

const loading = new Loading({
  $app,
  initialState: this.state.isLoading,
});
```

코드의 분량이 작아, 이 둘은 같은 곳에 넣어두었다.

**new ImageView()의 경우**

- $app, initialState를 추가했다. initialState의 경우 사진을 띄울 때 필요한 this.state.selectedFilePath를 넣어주었다.
- modalClose : 아까 구현되어 있지 않던 modalClose를 드디어 볼 수 있는 모습이다. 당연히 modal이 닫히면 selectedFilePath또한 필요 없기에 null로 넘겨줬다.

**new Loading()의 경우**

- $app, initialState를 넣어줬다. 현재, 로딩이 필요한 상황인지를 loading이 알아야 하기에 this.state.isLoading값을 넘겨준 모습이다.

ㅤ
ㅤ
ㅤ
ㅤ
ㅤ
ㅤ
ㅤ
ㅤ

## 회고

길지 않은 코드지만 많은 것을 얻을 수 있었던 것 같다.

앞서 말했었던 이 네가지에 대한 궁금증도 풀렸고

1. Vanila JavaScript를 통한 SPA 방식 만들기
2. 선언적 프로그래밍, 컴포넌트 추상화
3. 컴포넌트 간의 의존도 줄이기, 상태값을 통한 렌더링 구현
4. 나보다 더 실력좋은 사람은 어떻게 코드를 짰을까?

추가적으로, 코드를 진행하면서 나온 JS에 대한 지식들도 다시 복습할 수 있는 기회였다.

이 코드를 진행하면서 복기할 수 있었던 **js 개념의 경우 링크를 남겨둔 상태다.** 글을 잘 쓰는 편은 아니지만, 가서 한번 본다면 더 많을 걸 얻어 가실 수 있을 거라고 생각한다.

이번, 학교 기말고사가 끝나고, 면접도 끝나면 여유가 생길 때 이번 공부로 배운 SPA 및 컴포넌트 프로그래밍을 통해 토이 프로젝트를 하나 진득하게 만들어 보고자 한다.

좀 지루한 글이였지만, 많은 도움이 되었으면 좋겠다. 갑니다~
