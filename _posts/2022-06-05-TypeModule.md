---
publish: true
title: "JS - Module"
categories:
  - FrontEnd
tags:
  - JavaScript
---

## Script type = "module"은 뭘까?

## 모듈(module)이란?

모듈이란, 애플리케이션을 구성하는 개별적 요소로써 재사용 가능한 코드 조각을 말한다.

Javascript의 모듈에는 가장 중요한 특징을 하나 가지고 있다.

- 모듈은 자신만의 파일 스코프를 가지고 있다.

이게 별 말이 아닌 것 처럼 보이지만, 말 그대로 자신만의 파일 스코프를 갖는 모듈을 캡슐화되어 다른 모듈에서 접근할 수 없다.
=> 즉, 모듈은 개별적 존재로서 애플리케이션과 분리되어 존재한다.
ㅤ
ㅤ
ㅤ

이런 모듈에 접근하여 사용할 수 있는 방법이 존재한다. (ES6 기준으로 지원)

- export : 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개를 하는 것.
- import 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용 하는 것.

이런 식으로 모듈을 사용할 수 있는 방법이 존재한다.
ㅤ
ㅤ
ㅤ

또한, 모듈의 경우 기능별로 분리되어 개별적인 파일로 작성하다 보니 모듈을 통한 개발은 큰 장점들이 존재한다.

    1. 코드의 단위를 명확히 분리하여 애플리케이션 구성가능
    2. 재사용성이 좋아 개발효율성과 유지보수성이 올라감

ㅤ
ㅤ
ㅤ

## 모듈 in JS(ES6)

1. 모듈인 js파일을 script 태그로 html에서 불러오고 싶을 때

```
<script type = "module" src = "app.js"></script>
```

2.  export 키워드를 통해 모듈 재사용을 허가할 때

```
//일반 함수
export default function() hello{};

//화살표 함수 : 화살표 함수는 default 키워드를 못붙인다.
export const good = () => {};
```

- 여기서 default 키워드는 대표적으로 export 할 함수라는 뜻이다.

3. import 키워드를 통해 다른 모듈을 사용하고 싶을 때

```
//default 키워드가 붙은 모듈일 때
import hello from '파일 경로';

//default가 붙지 않은 모듈일 때
import { hello } from '파일 경로';
```

4. 살짝 꿀팁인데, 여러 모듈들을 한 js 파일에 묶어서 깔끔하게 불러오고 싶을 때

일단, 두 개의 모듈이 있다고 해보자

```
//module1.js
export default function Module1(){}
```

```
//module2.js
export default function Module2(){}
```

```
//index.js
export {default as Module1} from './module1.js';
export {default as Module2} from './module2.js';
```

이런 식으로, index js에서 다 합쳐 주면 깔끔하게 쓸 수 있다.

```
//App.js
import {Module1, Module2} from './index.js';
function App(){
	Module1();
	Module2();
}
```

App.js에서 모듈 한 줄로 import 문 처리가 되는 모습이다!
