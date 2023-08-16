# 고차 함수

고차함수 = '함수를 리턴하는' or '함수를 인자로 받는' 함수라고 생각하면 된다.
아래는 예시에서는 caculator가 고차함수다.

```javascript
const add = (a, b) => a + b;
function caculator(func, a, b) {
  return func(a, b);
}

add(3, 5);
calculator(add, 3, 5);
```

아래 onClickA, onClickB, onClickC는 같다.

```javascript
const onClickA = () => () => {
  console.log("hello");
};

const onClickB = () => {
  return () => {
    console.log("hello");
  };
};

const Hello = () => {
  console.log("hello");
};

const onClickC = () => Hello;
```

아래 코드애는 addEventListener에 onClick()으로 함수 리턴값이 들어가 있다.
틀린 코드일까? 틀리지 않다.

```javascript
const onClick = () => () => {
  console.log("hello");
};

document.querySelector("#header").addEventListener("click", onClick());
```

# Call Stack

CallStack은 '호출!!'일 때만 쌓이는 Stack이다. (선언문은 무시)
CallStack은 맨 밑에 anonymous가 쌓여있다(항상).

# Scope Chain

'호출!!'일 때만 쌓이는 Call Stack과 반대로 Scope Chain은 '선언!!'일 때만 형성된다.
Scope Chain도 마찬가지로 최상단은 anonymous다.

const와 let은 모두 블록 스코프를 가진다.
즉, 아래 코드의 switch, if, whiel, for 블록안에서만 유효

```javascript
switch(){
    ...const ...let
}

if(){
    ...const ...let
}

while(){
    ...const ...let
}

for(){
    ...const ...let
}
```

아래 환경에서의 스코프 체인은 다음과 같다.

c -> anonymous
a -> anonymous
b -> c -> anonymous

a에서 b는 호출이 불가하지만, a에 대한 Scope Chain에서는 b를 가지지 않지만,
b에서 a는 호출이 가능하다. anonymous에서 a를 가지기 때문

스코프 체인을 반대로 그려보면(선언 지도를 그려보면),

anonymous -> a,c
a(가진게 없음)
c -> b

```javascript
function c() {
  function b() {}
}

function a() {}
```

# lexical Scope

코드가 한번 실행되면, Scope Chain은 절대 바뀌지 않는다.
이러한 환경을 lexical Scope라고 한다. (코드가 작성되었을 때의 환경)

# Hoisting

호이스팅은 코드 실행전에(런타임) 컴파일 단계에서 단어 사전을 만드는 과정이라고 생각하면 된다.
그래서 호이스팅이 되는 코드들은 런타임 단계에서 Temporal Dead Zone에 있어도 호출이 가능하다.

## function, Arrow function에서의 호이스팅 차이

function은 호이스팅이 통째로 된다. (var는 선언문만 올라가는데 반해)

```javascript
console.log(a); // Error: Temporal Dead Zone
const a = () => {};

console.log(b); // function b는 호이스팅 되어 정상 출력
function b() {}
```

## const, let, var의 호이스팅 차이

var같은 경우는 호이스팅이 되더라도 선언 단계만 호이스팅이 된다.(즉, 할당이 되지 않음)
ES2016의 const, let으로 인해 호이스팅을 막는다!!(위로 올라가지 않는다.)

### 사실

const와 let은 호이스팅이 되지 않는다고 했지만, 실제로는 이루어지긴 한다.(개념적으로는 안이루어진다고 생각하면 편하다)

var와 let,const가 호이스팅 관련해서 다른 이유는 호이스팅 과정에서 읽어들이는 부분에 차이가 있기 때문인데,
변수 생성과정에서는 크게 1. 변수선언 2. 초기화(undefined) 3. 할당. 세 가지 과정이 있고
var는 호이스팅 과정에서 1.변수선언과 2.초기화까지 이루어지지만 let,const는 1.변수선언 까지만 이루어져서
스코프의 유효범위 처음부터 변수가 선언된 위치 전 까지 var는 참조하여 초기화 값(undefined)을 이끌어 낼수 있지만
let,const 는 초기화가 안되어있는 상태이기 때문에 참조에러가 발생하게 된다.
이러한 방식으로 참조할수 없는 구간을 일시적인 사각지대라 하여 TDZ(temporal dead zone)이라 한다

### var를 왜 쓰지 말아야 하나?

- 호이스팅으로 빡치게 한다.(더불어 선언문만 올라감)

# this

기억하자

- this는 기본적으로 window다.(DOM 기준)
  (strict 모드에서는 기본적으로 undefined)

```javascript
console.log(this); // window 객체

function A() {
  console.log(this); // window 객체
}
```

- this는 기본적으로 함수가 '호출!!'할 때 결정된다. (선언할 때가 아님! 즉, 렉시컬 환경과 반대)

```javascript
const obj = {
  name: "TaeSeong",
  sayName() {
    console.log(this.name);
  },
};

obj.sayName();
const Test = obj.sayName;
Test(); // window
```

- 함수가 호출할 때는 기본적으로 window, 객체.함수호출() 방식이면 객체를 가리킨다.

```javascript
function Human(name) {
  // 자바스크립트의 객체를 만드는 옛방식
  this.name = name;
}
new Human("TaeSeong"); // Human{name:"TaeSeong"}
```

```javascript
const obj = {
  name: "TaeSeong",
  sayName() {
    console.log(this.name); // TaeSeong
  },
  sayHello() => {
    console.log(`${this.name} says Hello`); // window, 화살표 함수는 영향을 받지 않는다.
  },
};

obj.sayName();
const Test = obj.sayName;
Test(); // window
```

- bind, call, apply 메서드로 함수에 대한 this를 바꿀 수 있다.

정리해보면 new, 객체.메서드(), call, bind, apply인 경우에만 this가 바뀐다. 그게 아니라면 모두 window(전역)라고 생각하면 된다.
하지만 화살표함수의 경우는 부모 스코프의 this를 물려받는다.(스코프이므로 렉시컬환경이라고 생각하면 된다.)

## 응용

1번

```javascript
const obj = {
  name: "TaeSeong",
  sayName() {
    console.log(this.name); // (1) TaeSeong, 객체.메서드호출()형태 이므로 this는 객체를 가리킨다.
    function innerSayName() {
      console.log(this.name); // (2) Window, 그냥 함수이므로 window를 가리킨다.
    }
    innerSayName(); // (3) 이떄 (2)의 this가 결정된다!!
  },
};

obj.sayName();
```

2번

```javascript
const obj = {
  name: "TaeSeong",
  sayName() {
    console.log(this.name); // (1) TaeSeong, 객체.메서드호출()형태 이므로 this는 객체를 가리킨다.
    const innerSayName = () => {
      console.log(this.name); // (2) TaeSeong, innerSayName의 부모 스코프의 this를 가지므로!! (화살표 함수는 부모 스코프의 this를 물려받는다)
    };
    innerSayName(); // (3) 이떄 (2)의 this가 결정된다!!
  },
};

obj.sayName();
```

3번, 2번 예제에서 아래 부분만을 가지고 각 this가 무엇인지 알 수 있을까?
-> 알 수 없다. this는 호출할 때! 결정되기 때문

```javascript
  sayName() {
    console.log(this.name);
    const innerSayName = () => {
      console.log(this.name);
    };
    innerSayName();
  }
```

예시로, 아래와 같이 해버리면 this는 window를 가리킨다.

```javascript
const sayN = obj.sayName;
sayN(); //
```

## bonus

obj1.sayName과 obj2.sayName의 차이는?

```javascript
const obj1 = {
  sayName() {
    };
};

const obj2 = {
  sayName: function () {
    };
};

```

obj1의 경우, es6 이후 적용되는 축약형 메서드이다.
축약형 메서드로 작성하면 프로퍼티 어트리뷰트인 [[constructor]]를 갖지 않는다.

## addEventListener의 this

버튼을 클릭하면 어떤 것이 출력될까?

```javascript
const header = document.querySelector(".button");
header.addEventListener("click", function () {
  console.log(this);
});
```

답은 button이 출력된다.

function (){} 형태라서 window라고 생각할 수도 있지만,

앞서 말했듯이 this는 호출될 때 결정된다. function (){}은 addEventListener 함수 내부안에서 실행되고, 그때의 this는 button이다. (이유는 모른다. addEventListener가 어떻게 생겼는지 모르기 때문에, addEventListener의 this가 엘리먼트를 가리킨다고 되어있다고 추측만 할 뿐)

함수는 일급객체이다. 이런 경우, function () {} 형태의 콜백 함수를 넘겨주게 되면 this는 추측할 수 없다. (해당 함수 내부 구조를 모를 때,)
하지만 이럴 때, () => {}과 같이 Arrow Function을 넘겨주면서 this 통제권을 가져올 수 있다.

## bind, apply, call

bind는 this만 바꿔서 새로운 함수를 만들어준다.
apply와 call은 this를 바꾸고 실행까지 해준다. (bind는 생성만)
apply와 call의 차이는 매개변수 할당 방식 (apply는 array, call은 순서 매개변수)

# CallBack Function

콜백 !== 비동기
콜백 = 동기 콜백 || 비동기 콜백

# Promise

실행 됐는데, 결과값을 나중에 쓸 수 있는 것

## 내 생각

Promise를 '콜백의 진화' 버젼이라고 잘못 생각하는 경우가 많은데, Promise는 '콜백의 진화'가 아니다.
콜백과 다르게 Promise의 가장 큰 장점은, '결과값을 나중에 쓸 수 있는 것'이다.
Promise가 없었다면, 아래와 같이 '1초 뒤 무조건 실행되는' 콜백함수만 사용해야할 것 이다.

```javascript
setTimeout(() => {}, 1000);
```

하지만 Promise를 사용한다면 아래와 같이 비동기 값을 '내가 사용하고 싶을 때' 사용할 수 있다.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout((결괏값) => {
    resolve();
  }, 1000);
});

딴짓();
딴짓();
딴짓();
딴짓();

promise.then(() => {
  console.log("");
});
```

## 내 생각 22

Async, Await의 단점을 보완하기 위해 Promise가 나왔다고 알려져있다.
그래서 무조건 Async, Await > Promise라고 생각하는 경우가 많다. (나 또한..)
하지만 Async, Await 조차, 앞서 말했던 '내가 사용하고 싶을 때' 사용할 수 있다는 Promise의 장점을 이기지 못한다.

```javascript
const p1 = axios.get("서버주소1");
const p2 = axios.get("서버주소1");
const p3 = axios.get("서버주소1");
const p4 = axios.get("서버주소1");
const p5 = axios.get("서버주소1");
const p6 = axios.get("서버주소1");

Promise.all([p1, p2, p3, p4, p5, p6]).then((result)=>());
```

위 코드를 완전히 await으로 대체 사용한다면, 더욱 지저분할 것이다. (하나라도 실패시 catch로 넘어간다는 것이 문제는 Promise.allSettled()로 해결 가능)

## 내 생각33

나는 무심코 'Promise = callBack Hell의 해결책'이 가장 큰 장점이라고 생각했는데,
지금 생각해보면 Promise의 가장 큰 장점은 'callBack의 결괏값을 바로 사용해야 하는 것에 대한 해결책' 일 수 있겠다.

# async

async 함수를 호출할 때는, 항상 then으로 리턴 값을 받아야한다.
혹은 await

```javascript
async function main(){
  const result = awiat promise;
  return 'TaeSeong'
}

main().then((name)=>...)
```

## for await of

노드 10부터 지원
resolve된 프로미스가 변수에 담겨 나온다.

```javascript
const promise1 = Promise.resolve("sucess1");
const promise2 = Promise.resolve("sucess2");

async () => {
  for await (promise of [primise1, primise2]) {
    console.log(promise);
  }
}();
```

# Macrotask, Microtask

아래 코드를 실행해보면, 우리는 화면에 "code", "promise", "timeout" 순서로 출력되는 것을 볼 수 있다.
이는 콜스택, 백그라운드, 매크로태스크, 마이크로태스크를 이해함으로써 순서를 이해할 수 있다.

```javascript
setTimeout(() => alert("timeout")); // 3

Promise.resolve().then(() => alert("promise")); // 2

alert("code"); // 1
```

1. 콜스택은 비동기 함수를 백그라운드에 보낸다.
2. 함수는 백그라운드에서 매크로태스크 또는 마이크로태스크로 할당된다. (어떤 함수인지에 따라)
3. 마이크로태스크 큐를 모두 비우고 매크로태스크 큐를 모두 비운다. (비운다는 것은 콜스택으로 보낸다는 의미)

자바스크립트 엔진은 크게 1, 2, 3의 동작을 반복하며 동작한다.

## 매크로태스크 큐와 마이크로태스크 큐 함수

### 콜백함수를 매크로태스크 큐에 넣는 함수

- setTimeout
- setInterval
- setImmediate
- requestAnimationFrame
- I/O
- - UI 렌더링

### 콜백함수를 마이크로태스크 큐에 넣는 함수

- process.nextTick
- Promise
- Object.observe
- MutationObserver

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```
