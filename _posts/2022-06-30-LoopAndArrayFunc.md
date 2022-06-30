---
published: true
title: "JS - for, Array.loopFunction"
categories:
  - FrontEnd
tags:
  - JavaScript
---

## 1. for ... in ... 과 for ...of... 의 차이점은 무엇일까?

평소에, 코딩테스트를 풀 때에도...

1. array의 인덱스 상관없이 값을 다 돌면서 볼 때
2. object의 key와 value를 하나씩 돌려볼 때

보통 이럴 때에, for in과 of를 잘 활용했던 것 같습니다.

하지만, 둘 다 쓰면서도 이 둘의 차이는 뭐고 어느상황에는 뭘 써야되지? 라는 고민이 많았습니다.

**아래는 MDN에서 정의한 for in과 for of 입니다.**

### for ... in

- forEach문은 객체의 열거 속성을 통해 지정된 변수를 반복합니다.
- **각각의 고유한 속성**에 대해, JavaScript는 지정된 문을 실행합니다.

### for...of

- for...of문은 각각의 **고유한 특성의 값**을 실행할 명령과 함께 사용자 지정 반복 후크를 호출합니다.
- **반복가능한 객체(배열, Map, Set, arguments 객체....)** 를 통해 반복하는 루프를 만듭니다.

여기서 볼 수 있는 둘의 키워드의 차이는 **고유한 속성**, **고유한 특성의 값**입니다.

이 키워드가 어떻게 적용되는지는 아래의 코드를 한번 볼까요?

```js
let arr = [3, 5, 7];
arr.foo = "hello";

for (let i in arr) {
  console.log(i); // logs "0", "1", "2", "foo"
}

for (let i of arr) {
  console.log(i); // logs "3", "5", "7"
}
```

- for ...in의 경우는 고유한 속성의 이름으로 돌기 때문에 arr.foo에도 접근합니다.
  - 한가지 더 팁이 있다면, for... in의 경우 value에 접근할 방법을 제공하지 않습니다.
- 하지만, for...of의 경우에는 값을 기반으로 접근하기 때문에 key인 foo가 나오지 않습니다.

추가로, key:value로만 구성되어 있는 object가 기준일 땐 어떨까요?

```js
let obj = { a: 1, b: 2, c: "수환" };

for (let i in obj) {
  console.log(i); //logs  "a", "b", "c"
}

for (let i of obj) {
  console.log(i); //TypeError: obj is not iterable
}
```

- in을 사용할 시, key에는 접근이 가능하기에 a,b,c를 출력합니다.
- of의 경우 에러가 났습니다.
  - 지금 object는 반복 가능한 객체가 아니기 때문에 오류가 난 것입니다.

### 그럼, obj은 of를 통해서 값을 못 보는 건가요??

아닙니다. Object의 메소드를 통해 재가공 후 해결이 가능합니다.
아래와 같은 종류가 있습니다.

1. Object.keys(원하는객체) => 키만 빼서 만들어진 배열반환
2. Object.values(원하는객체) => 값만 빼서 만들어진 배열반환
3. Object.entries(원하는객체) => [키,값]으로 빼서 만들어진 배열반환

요것들을 써서 for...of를 돌아볼까요?

```js
let obj = { a: 1, b: 2, c: "수환" };

for (let k of Object.keys(obj)) {
  console.log(k); //logs. "a", "b", "c"
}

for (let v of Object.values(obj)) {
  console.log(v); //logs. 1, 2, "수환"
}

for (let [k, v] of Object.entries(obj)) {
  console.log(k, v); //logs. "a", 1  ,  "b", 2   ,  "c","수환"
}
```

이런 식으로, Object의 내장함수를 통해서도 반복문을 돌 수 있습니다.

## 2. forEach, map, filter, reduce...

일단, 위의 함수는 Array의 고차함수입니다. 고차함수란 무엇일까요?
=> **고차함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말합니다.**

기존에 배열을 한번 돌려면 for, while을 써야 했는데 여기에서 **변수를 새로 만들어야 되는 상황이 필연적으로 발생했습니다.** (for의 let i를 만들기 같은...)

또한, 한줄로 반복문을 구현하고 싶은데 코드줄을 좀 잡아먹기도 하죠 ㅋㅋㅋ...

그렇기에, 기존 반복문에서의 **변수의 사용을 억제**하고 **코드를 단순화**하기 위해서 밑에서 배울 함수들을 사용하게 된 것입니다.

자 하나씩 살펴볼까요?

### 1. Array.prototype.forEach

코드를 보면 매우 빠르게 이해될 것 같습니다. 코드를 볼까요?

```js
const numbers = [1, 2, 3];
const pows = [];

for (let i = 0; i < numbers.length; i++) {
  pows.push(numbers[i] ** 2);
}

console.log(pows);
```

위의 코드는 일반 for문을 통해서 numbers의 각 값들에 2를 곱한 값들을 pows에 push해주는 코드입니다.
이 코드는 forEach를 통해 더 깔끔하게 짤 수 있습니다. 한번, forEach를 쓴 코드를 봐볼까요?

```js
const numbers = [1, 2, 3];
const pows = [];

numbers.forEach((item) => pows.push(item ** 2));
console.log(pows);
```

코드가 매우매우매우 깔끔하게 바뀌었습니다. forEach를 좀 더 봐볼까요?
forEach는 대락 이렇게 생겼습니다.

```js
Array.forEach((요소값, index, this) => {동작할_구문;});
```

위의 코드에는 인수를 하나만 받았지만, 사실 **세 개의 인수들을 얻을 수 있습니다.**

1. 첫 째는 당연하게도, **요소값**입니다. 위에서는 1,2,3을 받았습니다.
2. 두 번째는 **인덱스**입니다. 아마 받는다면 0,1,2를 받겠네요.
3. 세 번째는 **this**입니다. 여기서의 this는 forEach를 호출한 배열 Array입니다.

또한, **화살표 함수 옆에 동작할 구문들을 넣어줍니다.**

- 한 줄일 경우는 위의 코드처럼 블록을 쳐줄 필요 없습니다.
- 하지만, 여러 줄일 경우에는 위의 forEach 해부도처럼 블록을 만들어야겠죠?

더 추가적으로 알아야 할 것이 있다면....

1. **forEach의 내부는 사실 for문이 수행됩니다.** 다만, 코드를 숨긴 것이지요.
2. **forEach는 break, continue를 못 씁니다.**
3. **forEach의 성능은 순수 for보다는 나쁩니다.** 그래도, 코드가 깔끔해지죠.

### 2. Array.prototype.map

분명 js에는 hashMap을 만드는 new Map()이 있었죠? 그 맵이랑은 다릅니다. 조심!

바로 코드로 볼까요?

```js
const numbers = [1, 4, 9];
//Math.sqrt(item) === item ** 2
const pows = numbers.map((item) => Math.sqrt(item));
console.log(pows);
```

위에서 forEach를 볼 때 썼던 코드와 아예 똑같은 코드입니다. 하지만, 한 줄이 더 줄었네요??

일단, 왜 이런 상황이 발생했는지 설명하기 전에 map도 한번 까볼까요?

```js
Array.map((요소값,index,this) => {동작할_구문; return 값;});
```

보면, 위의 forEach와 인수도 똑같습니다. 하지만, 차이가 존재하는데요.
**map은 현재 보는 배열과 길이가 똑같은 배열에 각 인덱스값을 동작할\_구문에서 반환한 값을 push해줍니다.**

**그렇기, 때문에 위의 코드를 자세히 보면 식별자 pows로 반환된 배열을 받아주는 모습을 볼 수 있죠!**

### 3. Array.prototype.filter

다음은 filter입니다. 뭔가, 이름부터 어떤 조건으로 거른다는 느낌이 나죠?
말 그대로의 의미를 가지고 있는 함수입니다. 코드를 볼까요?

```js
const numbers = [1, 2, 3, 4, 5];

const odds = numbers.filter((item) => item % 2 === 1);
console.log(odds); //logs. [1,3,5]
```

위의 코드는 numbers의 요소들 중 홀수들만 남긴 새로운 배열을 만들고, console.log로 찍어주는 코드입니다.

해부도는 아래와 같습니다.

```js
Array.filter((요소값,index,this) => {동작할_구문; return 조건;});
```

위의, forEach, map과 인수는 비슷합니다. 또한, map처럼 새로 만들어진 배열도 반환해줍니다.

다만, 차이점이 있죠. **return을 요소값을 새 배열에 push할 조건으로 넣습니다.**

위의 코드를 보면 한줄 적힌 구문은 {return item%2===1}와 같습니다.
=> **그렇기에, 각 item을 2로 나눴을 때 나머지값이 1이면 홀수이고 이런 홀수인 item만 odds에 push되는 것이지요.**

### 4. Array.prototype.reduce

reduce의 경우 위의 것들과 성질이 살짝 다릅니다. 코드부터 보겠습니다.

```js
const sum = [1, 2, 3, 4].reduce((prev, cur, index, array) => prev + cur, 0);

console.log(sum); //logs. 10
```

위의 코드를 보면, 뭔가 1+2+3+4를 해서 10을 받고 출력하는 것 같죠?
일단, reduce도 분해해볼까요?

```js
Array.reduce((accumulator, currentValue, index, this)=> 실행문, 초기값);
```

currentValue, item, this는 위의 map, forEach와 동일합니다.
accumulator의 경우 매 턴마다 달라지는 값입니다. - **accumulator는 실행문에서 반환한 값으로 매 턴마다 갱신됩니다.** - 또한, **accumulator의 최초값은 실행문 옆에 초기값**입니다.

그렇기 때문에, 위의 코드의 동작과정은 이렇게 볼 수 있는거죠.

1. cur = 1 일때 , prev = 1 + 0
2. cur = 2 일때 , prev = 2 + 1
3. cur = 3 일때 , prev = 3 + 3
4. cur = 4 일때, prev = 6 + 4 = 10이 나옴.

### 5. 요약

1. forEach는 배열을 한번 돕니다.
2. map은 배열을 한번 돌며, 새로운 배열을 반환합니다.
3. filter는 배열을 한번 돌며, 조건에 맞는 값만으로 구성된 새로운 배열을 반환합니다.
4. reduce는 배열을 한번 돌며, 누적 계산된 하나의 값을 반환합니다.

## Reference

Javascript deep dive(책)
https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Loops_and_iteration
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
