![](https://i.imgur.com/LOqoalW.png)

본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# ES6 함수의 추가 기능

## 함수의 구분

```javascript
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

tip) 인스턴스를 생성할 수 있는 함수 객체를 constructor, 인스턴스를 생성할 수 없는 함수 객체를 non-constructor라고 부른다

```javascript
// 프로퍼티 f에 바인딩된 함수는 callable이며 constructor다.
var obj = {
  x: 10,
  f: function () {
    return this.x;
  },
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

```javascript
// 콜백 함수를 사용하는 고차 함수 map. 콜백 함수도 constructor이며 프로토타입을 생성한다.
[1, 2, 3].map(function (item) {
  return item * 2;
}); // -> [ 2, 4, 6 ]
```

위 세 예제 코드에서 볼 수 있듯이, ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다.

**ES6 이전의 함수 특징**

- ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 몰론 생성자 함수로서 호출할 수 있다.(callable이면서 constructor)
- 객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며, 프로토타입 객체도 생성한다는 것을 의미한다.
- 함수에 전달되어 보조함수의 역활을 수행하는 콜백 함수도 마찬가지이다. 콜백 함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성한다.

이처럼 ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다. 이는 혼란스러우며 실수를 유발할 가능성이 있고 성능에도 좋지 않다.

따라서 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.

| ES6 함수의 구분    | constructor | prototype | super | arguments |
| ------------------ | ----------- | --------- | ----- | --------- |
| 일반 함수(Normal)  | O           | O         | X     | O         |
| 메서드(Method)     | X           | X         | O     | O         |
| 화살표 함수(Arrow) | X           | X         | X     | X         |

## 메서드

ES6 사양에서는 메서드에 대한 정의를 명확하게 규정하였다. ES6 사양에서는 인스턴스를 생성할 수 없는 non-constructor를 메서드라 정의하였다.

```javascript
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다. **super 참조는 내부 슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조**하므로 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드는 super 키워드를 사용할 수 있다. (ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다. [[HomeObject]]를 갖지 않기 때문이다.)

```javascript
const base = {
  name: "Lee",
  sayHi() {
    return `Hi! ${this.name}`;
  },
};

const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  },
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

```javascript
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드가 아니다.
  // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError: 'super' keyword unexpected here
    return `${super.sayHi()}. how are you doing?`;
  },
};
```

이처럼 ES6 메서드는 본연의 기능을(super) 추가하고 의미적으로 맞지 않는 기능은 제거했다.
따라서 메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.

## 화살표 함수

### 함수 정의

화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의한다.
호출 방식은 기존과 동일

```javascript
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

### 매개변수 선언

매개변수가 여러 개인 경우 소괄호 ()안에 매개변수를 선언한다.

```javascript
const arrow = (x, y) => { ... };
```

매개변수가 한 개인 경우 소괄호 ()를 생략할 수 있다.

```javascript
const arrow = x => { ... };
```

매개변수가 없는 경우 소괄호 ()를 생략할 수 없다.

```javascript
const arrow = () => { ... };
```

### 함수 몸체 정의

함수 몸체가 하나의 문으로 구성되면 함수 몸체의 중괄호 {}를 생략할 수 있다.
하지만 표현식이 아니면 에러 발생(표현식이 아니면 반환할 수 없기 때문)

```javascript
// concise body
const power = (x) => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
// block body
const power = (x) => {
  return x ** 2;
};

const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

따라서 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.

```javascript
const arrow = () => {
  const x = 1;
};
```

객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸 주어야 한다. 그렇지 않으면 함수 몸체를 감싸는 중괄호 {}로 잘못 해석한다.

```javascript
const create = (id, content) => ({ id, content });
create(1, "JavaScript"); // -> {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => {
  return { id, content };
};
```

```javascript
// { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => {
  id, content;
};
create(1, "JavaScript"); // -> undefined
```

화살표 함수는 일반적인 함수 표현식보다 표현이 간결하고 가독성이 좋다.

```javascript
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map((v) => v * 2); // -> [ 2, 4, 6 ]
```

### 화살표 함수와 일반 함수의 차이

- 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.

```javascript
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError: Foo is not a constructor
```

```javascript
const Foo = () => {};
// 화살표 함수는 prototype 프로퍼티가 없다.
Foo.hasOwnProperty("prototype"); // -> false
```

- 화살표 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
  따라서 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.
  만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this, arguments, super, new.target 바인딩이 없으므로 스코프 체인 상에서 가장 가까은 상위 함수 중에서 화살표 함수가 아닌 함수의 this, arguments, super, new.target을 참조한다.

### this

22장 this 학습 후 보완

## Rest 파라미터

22장 this 학습 후 보완

## 매개변수 기본값

JS엔진은 매개변수의 개수와 인수의 개수를 체크하지 않아, 다음 예제와 같이 의도치 않은 결과가 나올 수 있다.

```javascript
function sum(x, y) {
  return x + y;
}

console.log(sum(1)); // NaN 인수가 전달되지 않은 매개변수의 값은 undefined다.
```

따라서 다음 예제와 같이 매개변수에 인수가 전달되었는지 확인하여 인수가 전달되지 않은 경우 매개변수 기본값을 할당할 필요가 있다. 즉, 방어 코드가 필요하다.

```javascript
function sum(x, y) {
  // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당한다.
  x = x || 0;
  y = y || 0;

  return x + y;
}
```

ES6에서는 매개변수 기본값을 도입했다.

매개변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.
(앞서 살펴본 Rest 파라미터에는 기본값을 지정할 수 없다.)

```javascript
console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
26 - 59;
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```
