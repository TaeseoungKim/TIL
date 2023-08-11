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

## lexical Scope

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
    console.log(this.name); // Taeseong
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

```javascript
const obj = {
  name: "TaeSeong",
  sayName() {
    console.log(this.name); // Taeseong
    innerSayName() => {
    console.log(this.name); // Window
  },
  },

};

obj.sayName();
const Test = obj.sayName;
Test(); // window
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
