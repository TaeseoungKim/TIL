호이스팅에 대한 오해

## 호이스팅이란?

자바스크립트에서는 변수 선언 구문에 도달하지 않더라도, 해당 변수를 사용할 수 있다. (몰론 값을 할당되어있지 않다)
이러한 이유는, 파일을 실행하기전 자바 인터프리터에 의해, 모든 변수 선언문을 가지고 하나의 딕셔너리를 만들어 놓기 떄문이다.
그래서 선언 구문에 도달하기 전부터 사용할 수 있는 것이다.

하지만, let과 const와 같은 경우는 선언전에 사용을 하게 되면 에러가 출력된다.
Let과 const는 변수 선언전에 사용을 할 수 없도록 추가적인 기능을 구현했기 때문이다.
Var, let, const 모두 호이스팅이 일어난다.

또한, let과 const는 짜증나는 var의 function scope를 벗어나기 위해, block scope를 가진다. 즉, if문 안에서 선언된 const 변수는 if문 안에서만 사용할 수 있다
헷갈리지 말아야 할것은, let과 const는 블락 내부에서 외부를 참조할 수 있다는 것이다. (외부에선 내부로 참조 불가능)

## 고차함수란? 함수 안에서 다른 함수를 리턴하는 함수

아래 코드의 두 onClick은 같다.

```javascript
const onClick = () => () => {
  console.log("hello");
};

const onClick = () => {
  return () => {
    console.log("hello");
  };
};
```

아래의 코드는 보편적으로 고차 함수를 사용하는 예시이다.

```javascript
const onClick = () => () => {
  return () => {
    console.log("hello");
  };
};

document.querySelector(`#header`).addEventListener(`click`, onClick());
```

그렇다면, 아래의 두 onClick 함수 중, 옳은 event 매개변수 위치는 무엇일까?

```javascript
const onClick = (event) => () => {
  // 오답
  return () => {
    console.log("hello");
  };
};

const onClick = () => (event) => {
  // 정답
  return () => {
    console.log("hello");
  };
};

document.querySelector(`#header`).addEventListener(`click`, onClick());
```

```javascript
const styled = (aElement) => {
  const el = document.createElement(aElement);
  return (args) => {
    const styles = args[0];
    el.style = styles;
    return el;
  };
};

// 아래와 같이, 변수 뒤에 벡틱을 붙혀주면 변수를 함수로 간주하고 함수 실행문이 된다. (벡틱 내부 내용은 argument[0]으로 들어간다.)
const title = styled("h1")`
  background-color: red;
  color: blue;
`;
```

### 벡틱과 고차함수를 이용한 Styled Component 구현

## function 선언 방법에 따른 this

일반적인 Function 선언 방식 => 함수가 호출될 때, this가 결정된다. window를 가리킨다.
Arrow Function 선언 방식 => 함수가 실행될 때, this가 결정된다. 상위 this를 물려받는다.

```javascript
const button = document.querySelector(`button`);

button.addEventListener(`click`, function () {
  this.style.backgroundColor = "red"; // 정상작동
});

button.addEventListener(`click`, () => {
  this.style.backgroundColor = "red"; // Error 발생, this는 window 객체를 가르킨다.
});
```

위의 예시를 보면, function 선언 방법에 따라 this 키워드가 참조하는 것은 다르다.
function 키워드를 이용해 선언을 하면 this는 자신이 속한 객체에 대한 참조를 갖지만, Arrow Function 내부의 this는 상위 계층의 객체 참조를 갖는다.

왜이런 것일까?
정답: 화살표 함수에는 this가 아예 없기 때문이다.
화살표 함수에는 this가 아예 없기 떄문에, 그 상위 환경에서의 this를 참조하는 것이다.

addEventListener()의 콜백함수

```javascript
const button = document.getElementById("myButton");

button.addEventListener("click", () => {
  console.log(this); // Window
  this.innerHTML = "clicked";
});

button.addEventListener("click", function () {
  console.log(this); // button 엘리먼트
  this.innerHTML = "clicked";
});
```

원래 addEventListener의 콜백함수에서는 this에 해당 이벤트 리스너가 호출된 엘리먼트가 바인딩되도록 정의되어 있다. 이처럼 **이미 this의 값이 정해져있는 콜백함수**의 경우, 화살표 함수를 사용하면 기존 바인딩 값이 사라지고 상위 스코프(이 경우엔 전역 객체)가 바인딩되기 때문에 의도했던대로 동작하지 않을 수 있습니다. 물론 상위 스코프의 속성들을 쓰려고 의도한 경우라면 사용할 수 있습니다.

1. Arrow Function의 경우, this 자체가 없기 떄문에 상위 this를 물려받는다.

대체로 this는 위의 개념을 따르지만, this의 값이 정해져 있는 경우들이 있다. (일부는 Javascript 설계상의 오류라 한다)

함수에서 사용될 때, this는 전역 객체를 가리킨다.
객체에 속한 메소드의 내부함수에서 사용될 때 this 는 전역 객체를 가리킵니다.(설계상의 오류)

```javascript
const nico = {
  name: "NICO",
  age: 24,
  addYear: () => {
    this.age++;
  },
};

console.log(nico); // 24
nico.addYear();
nico.addYear();
console.log(nico); // 24
```

```javascript
const nico = {
  name: "NICO",
  age: 24,
  addYear() {
    this.age++;
  },
};

console.log(nico); // 24
nico.addYear();
nico.addYear();
console.log(nico); // 26
```

## Array

### Array.from

Array.from은 arrayLike Object를 가지고, Array로 만드는 역할을 수행하는 메서드이다.

```javascript
const buttons = document.querySelectorByClass(`.btn`);

buttons.forEach((btn) => btn.addEventListener(`click`, brrr)); // 오류. buttons는 Array가 아닌 Array Like Object (Node List)이기 때문에, forEach 메서드를 가지고 있지 않다.

Array.from(buttons).forEach((btn) => btn.addEventListener(`click`, brrr)); // 이런식으로 사용해야 한다.
```

## Object Destructuring

```javascript
const settings = {
  notifications: {
    follow: true,
    alert: true,
    sucks_Name: false,
  },
  color: { dark: true },
};

// follow 값 만을 사용하고 싶을 때는, 아래와 같이 사용할 수 있다.
const {
  notifications: { follow },
} = settings;
console.log(notifications); // Error 발생, 위 구문은 notifications를 변수로써 가져오는 것이 아니다. notifications는 경로를 알려주는 것일 뿐,

// notifications Property가 없을 경우, follow Property가 없을 경우를 대비해서, 기본값을 설정해주고 싶을 때는 아래와 같이 작성해주면 된다.
const { notifications: { follow = false } = {} } = settings;
// 구조분해할당을 할 때, Property의 이름을 바꿔서 사용할 수 있다. (실제로 바꾸는 것이 아닌, 바꾸고 싶은 이름을 가진 변수에 할당하는 개념)
const { notifications: { sucks_name: coolName } = {} } = settings;
```

## Array Destructuring

```javascript
const days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

// 객체 구조분해할당과 마찬가지로 기본값을 설정해줄 수 있다.
const [mon, thu, wed = "Wed"] = days;
```

## Spread, Rest

### Spread

Spread를 객체 선언문 안에서도 사용할 수 있다.

```javascript
const lastName = prompt("LastName을 입력하세요");
const user = {
  username: "nico",
  age: 24,
  ...(lastname !== "" && { lastname }),
};
```

### Rest

아래와 같이 Rest을 활용할 수 있다.

```javascript
const infiniteArgs = (firstOne, ...rest) => {
  console.log(firstOne); // "3" 출력
  console.log(rest); // 2, 3, 5, 1, 2, "1235342", "!123123123" 등이 출력
};

infiniteArgs("3", 2, 3, 5, 1, 2, "1235342", "!123123123");
```

## for 문을 자제하고, for ... of를 사용하자.

for문, forEach는 **배열**에서만 사용이 가능하다.
반면, for ... of은 배열, 객체, map, set 등 많은 iterable한 자료형에서 사용할 수 있다.
또한, let과 const를 사용할 수 있는 것도 장점이다.
또한, forEach와 비교해서 break문을 쓸 수 있는 것도 장점이다.

## promise, async, await

then이 promise가 resolve 되기를 기다리는 것처럼, await도 마찬가지

## parallel await

```javascript
const getMoviesAsync = async () => {
  const [firstPromiseValue, secondPromiseValue] = await Promise.all([
    fetch("https://blah....1"),
    fetch("https://blah....2"),
  ]);

  const [first, second] = await Promise.all([
    firstPromiseValue.json(),
    secondPromiseValue.json(),
  ]);
};
```

## class

Class is bluePrint.
객체에 대한 뼈대이다.

## Set vs Map

Set = 고유한 '값'들을 가진 자료형
Map = '키'-'값' 쌍들을 가진 자료형

## generators

```javascript
function* testGenerator() {
  for (friend of friends) yield next;
}

const result = testGenerator();
console.log(result.next); // 1 friend
console.log(result.next); // 2 friend
console.log(result.next); // 3 friend
console.log(result.next); // 4 friend
console.log(result.next); // 5 friend
```

## Proxy

Proxy는 객체에 대한 이벤트를 가로챈다.
예로, 아래는 객체에 대한 get과 set을 가로챈다.

```javascript
const userObj = {
  username: "nico",
  age: 12,
  password: 1234,
};

const userFilter = {
  get: () => {
    return "nothing";
  },

  set: () => {
    console.log("Somebody wrote something");
  },
};

const filteredUser = new Proxy(userObj, userFilter);
```

## import VS require

1. require (JavaScript - CommonJS):
   require는 주로 Node.js 환경에서 사용되는 CommonJS 모듈 시스템에서 사용되는 키워드이다. CommonJS는 서버 측 JavaScript 환경에서 모듈을 로딩하고 재사용하기 위한 표준이다.

```javascript
const module = require("module-name");
```

require는 '동기적'으로 작동하며, 모듈을 가져올 때 해당 모듈의 내용이 실행되고 그 결과가 할당된다.

2. import (JavaScript - ES6 Modules):
   import는 ECMAScript 6 (ES6) 이후의 JavaScript에서 사용되는 모듈 시스템이다. import를 사용하면 모듈을 '비동기적'으로 가져올 수 있고, '더 모듈화된 구조'를 제공한다.

```javascript
import module from "module-name";
```

import는 상대적으로 모던한 방식으로, '모듈의 내용이 필요한 시점에 가져와 사용한다. 브라우저에서도 점차 지원이 늘고 있다.

요약하면, require는 주로 Node.js와 같은 CommonJS 환경에서 사용되며 동기적으로 작동하며, import는 ES6 모듈 시스템에서 사용되며 비동기적으로 작동하고 모던한 모듈화 접근 방식을 제공한다.

## ?? (널 병합 연산자)

?? 연산자는 널 병합 연산자(nullish coalescing operator)로 알려져 있다.
이 연산자는 주로 값이 null 또는 undefined인 경우에 대체값을 지정하는 데 사용된다.

?? 연산자는 왼쪽 피연산자를 평가하고, 이 값이 null 또는 undefined인 경우에만 오른쪽 피연산자를 반환한다. 그렇지 않으면 왼쪽 피연산자의 값을 반환한다.

```javascript
var x = null;
var y = "기본값";

var result = x ?? y;
console.log(result); // "기본값"
```

위의 코드에서 x 변수는 null이므로 ?? 연산자는 오른쪽 피연산자인 "기본값"을 반환한다.
반면에, 만약 x 변수가 null이 아닌 값을 가지고 있다면

```javascript
Copy code
var x = "다른 값";
var y = "기본값";

var result = x ?? y;
console.log(result); // "다른 값"
```

이 경우에는 x 변수의 값이 반환된다. ?? 연산자는 null 또는 undefined만을 체크하며, 다른 falsy한 값들 (예: 빈 문자열, 숫자 0, false)은 해당 값 자체를 반환한다.

### OR 연산자(||)

널 병합 연산자(??)와 유사한 연산자로는 OR 연산자(||)가 있습니다. 이 두 연산자는 비슷한 상황에서 사용될 수 있지만 동작에 있어서 차이가 있다.
널 병합 연산자 (??): 이 연산자는 null 또는 undefined인 경우에만 오른쪽 피연산자를 반환한다. 다른 falsy한 값(빈 문자열, 숫자 0, false)은 왼쪽 피연산자로 간주한다.

```javascript
Copy code
var x = null;
var y = "기본값";

var result = x ?? y;
console.log(result); // "기본값"
```

OR 연산자 (||): 이 연산자는 왼쪽 피연산자가 falsy한 값(예: null, undefined, 빈 문자열, 숫자 0, false)일 때 오른쪽 피연산자를 반환합니다. 그렇지 않으면 왼쪽 피연산자의 값을 반환합니다.

```javascript
Copy code
var x = null;
var y = "기본값";

var result = x || y;
console.log(result); // "기본값"
```

주요 차이점은 ?? 연산자가 명시적으로 null 또는 undefined 여부를 체크하는 반면, || 연산자는 falsy한 값 여부를 체크합니다. 따라서 || 연산자를 사용할 때는 주의해야 하며, 예기치 않은 동작을 방지하기 위해 명시적으로 ?? 연산자를 사용하는 것이 좋다.
