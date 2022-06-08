---
published: true
title: "JS - callBack, Promise, async/await"
categories:
  - FrontEnd
tags:
  - JavaScript
---

## 이 셋은 어디에 쓰나요?

=> 보통, 비동기 프로그래밍을 구현하기 위해 사용된다.

오늘은 비동기 프로그래밍을 위해 CallBack Function, Promise, async/await가 뭐고, 뭘 쓰면 되는지 간단하게 보자.
(Generator도 존재하는 것으로 알고 있으나 다 담지 못했다... 차후에 더 공부해볼 예정이다.)

먼저 CallBack Function과 Promise를 비교해보자.

## Promise vs Callback function

CallBack 함수란?

- **다른 함수의 전달인자(argument)로 넘겨주는 함수**를 말한다.
- 또한, CallBack 함수로 무조건 비동기 프로그래밍만 하는 건 아니다. **콜백 함수를 써서 비동기 프로그래밍을 할 수도 있구나 라고 이해함이 옳다.**

아래는 CallBack 함수를 통한 간단한 비동기 프로그래밍의 구현이다.

```js
findUserAndCallBack(1, function (user) {
  console.log("user:", user);
});

function findUserAndCallBack(id, cb) {
  setTimeout(function () {
    console.log("waited 0.1 sec.");
    const user = {
      id: id,
      name: "User" + id,
      email: id + "@test.com",
    };
    cb(user);
  }, 100);
}
```

### 다만, Callback을 썼을 때에는 Promise 방식보다 단점이 여러 개 존재한다.

1. **CallBack hell**
   => 간단한, 비동기 구현이면 상관이 없긴 한데, 계속 받은 결과값을 꼬리물기로 활용하면 지옥의 코드가 탄생한다. 아래와 같은 코드를 CallBack hell이라고 한다.
   ![image](https://user-images.githubusercontent.com/60723373/172058494-3132ba29-cee0-443f-8bbc-5241e07e147b.png)
   (도망가고 싶다.)
   ㅤ
   ㅤ
   ㅤ

2. **에러 처리가 곤란하다는 것이다.**
   예를 들어, 이런 코드가 있다고 해보자.

```js
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  console.log("캐치된 에러", e);
}
```

놀랍게도 catch 문이 실행되지 않는다!

예외는 함수를 호출한 상위로 거슬러 전파된다. 지금, setTimeout 내부의 에러를 발생시키는 함수는 try-catch 안에서 단순히 정의된 상황이다.

**실제로 함수는 setTimeout에 의해서 실행된다. 그렇기에, try-catch 안에서 발생한 오류가 아니므로 예외를 감지할 수 없는 상황이 발생한 것이다.**
ㅤ
ㅤ
ㅤ
ㅤ
ㅤ
ㅤ

**자 이제, 반대로 Promise를 보자.**

Promise의 경우 성공했다는 **fullfilled**, 실패했다는 **rejected**, 진행중이라는 **pending**이라는 3가지 상태가 존재한다.

또한, then과 catch를 이 것들과 연계하여 비동기 처리를 구성하는데 성공하면 **then을 통해 코드를 내려가면서 계속 값을 가져가기 때문에 복잡한 callback hell을 방지할 수 있다. 그리고 catch를 통해 에러 핸들링또한 가능**하다.

아래의 코드는 두 가지를 담고 있는 코드이다.

1. 기본적인 Promise 사용 코드
2. Promise.catch를 통한 에러처리 코드

```js
const condition = true;
//resolve는 성공했을 때 실행되는 함수이다.
//reject는 실패했을 때 실행되는 함수이다.
const promise = new Promise((resolve, reject) => {
  if (condition) {
    resolve("resolved");
  } else {
    reject("rejected");
  }
});

promise
  .then((res) => {
    console.log(res);
  })
  .catch((error) => {
    console.error(error);
  });
```

### 결론적으로는, Callback의 단점을 해결한 Promise가 더 좋다고 볼 수 있다.

ㅤ
ㅤ
ㅤ
ㅤ
ㅤ
ㅤ

## async/await vs promise

async/await의 경우 위의 두 방법의 단점을 어느정도 해소하고자 어느정도 최근에 나온 비동기 프로그래밍 문법이다.

아래는, 자주쓰는 async/await로 구현한 request 코드이다.

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

둘의 차이는 다음과 같다.

- **에러 핸들링**
  - `Promise` 를 활용할 시에는 `.catch()` 문을 통해 에러 핸들링이 가능하지만, `async/await` 은 에러 핸들링 할 수 있는 기능이 없어 `try-catch()` 문을 활용하는 모습을 위의 코드로 볼 수 있다.
- **코드 가독성**
  - `Promise`는 `.then()` 이 쭉 내려가는, "then 지옥"이 발생할 수도 있다.
  - 코드가 길어지면 길어질수록, `async/await` 를 활용한 코드가 가독성이 좋다.
  - `async/await` 은 비동기 코드가 동기 코드처럼 읽히게 해준다. 코드 흐름을 이해 하기 쉽다.

당장 위의 코드도, fetch를 통해 받은 데이터를 상수 res에 저장하고 동기 코드처럼 쭉 내려가면서 코드가 진행된다.

이런 점들이 있다보니, 필자의 경우 비동기 코드를 짜야 할 때 async/await 방식을 통해 구현하고 있다.
