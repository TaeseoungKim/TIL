본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# Object Literal

## 객체란?

자바스크립트는 **객체기반의 프로그래밍 언어이며, 구성하는 거의 모든것이 객체다.**

원시 타입(Primitive type) - 하나의 값만을 나타낸다. **(변경 불가능)**
객체 타입(Obeject type) - 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 **복학접인 자료구조**다. **(변경 가능)**

객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키(Key)와 값(Value)으로 구성된다.
자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있다.

- **tip) 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드(method) 부른다.**
- 자바스크립트의 객체는 함수와 밀접한 관계를 가진다. 함수로 객체를 생성하기도 하며 함수 자체가 객체이기도 하다. 자바스크립트에서 함수와 객체는 분리해서 생각할 수 없는 개념이다.

```javascript
var counter = {
  num: 0,
  increase: function () {
    this.num++;
  },
};
```

이처럼 객체는 프로퍼티와 메서드로 구성된 집합체다.

- 프로퍼티: 객체의 상태를 나타내는 값(data)
- 메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

## 객체 리터럴에 의한 객체 생성

클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스를 생성하는 방식으로 객체를 생성한다.

- 인스턴스: 클래스에 의해 생성되어 **메모리에 저장된 실체**, 객체가 메모리에 저장되어 실제로 존재하는 것
- 클래스: 인스턴스를 생성하기 위한 템플릿

**자바스크립트는 프로토타입 기반 객체지향 언어**로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지향한다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

리터럴?: 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
JS의 객체 생성 방법 중, 가장 일반적인 방법은 **객체 리터럴**이다.

```javascript
var person = {
    key: value, ...
}
```

객체 리터럴의 중괄호는 코드 블록을 의미하지 않는다. 코드 블록의 닫는 중괄호 뒤에는 세미콜론을 붙이지 않는다.
객체 리터럴의 닫는 중괄호에는 세미콜론을 붙인다.

## 프로퍼티

프로퍼티는 키와 값으로 구성된다.

프로퍼티 키

- 식별자 네이밍 규칙 준수 O: 식별자 네이밍 규칙을 준수하는 이름, 즉 자바스크립트에서 사용 가능한 유요한 이름인 경우 따옴표를 생략할 수 있다.
- 식별자 네이밍 규칙 준수 X: 식별자 네이밍 규칙을 준수하지 않는 이름, 반드시 따옴표를 사용해야한다.

```javascript
var person = {
  firstName: "Ung-mo",
  "last-name": "Lee",
};
```

빈 문자열을 프로퍼티 키로 사용해도 에러는 발생하지 않지만, 권장하지 않는다.

```javascript
var foo = {
  "": "", // 빈 문자열도 프로퍼티 키로 사용할 수 있다.
};

console.log(foo); // {"": ""}
```

프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.
예를 들어, 프로퍼티 키로 숫자 리터럴을 사용하면 내부적으로는 문자열로 변환된다.

```javascript
var foo = {
  0: 1,
  1: 2,
  2: 3,
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

var, function과 같은 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않는다. 하지만 권장하지 않는다.
**이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.**

```javascript
var foo = {
  name: "Lee",
  name: "Kim",
};

console.log(foo); // {name: "Kim"}
```

## 메서드

자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용할 수 있다고 했다.
**당연히 함수도 프로퍼티 값으로 사용할 수 있다.**
앞서 말했듯이, 함수를 프로퍼티 값으로 사용하면 **메서드**라고 부른다.

```javascript
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () {
    // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  },
};

console.log(circle.getDiameter()); // 10
```

tip) 메서드 내부에서 사용한 this 키워드는 객체 자신(위 예제에서는 circle 객체)을 가리키는 **참조변수**다.

## 프로퍼티 접근

프로퍼티에 접근하는 방법은 다음과 같이 두 가지다.

- 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법
- 대괄호 프로퍼티 접근 연산자([...])를 사용하는 대괄호 표기법

프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름이면, 마침표 표기법과 대괄호 표기법을 모두 사용할 수 있다.
**대괄호 표기법을 사용하는 경우 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열**이어야 한다.
따옴표로 감싸지 않으면, 자바스크립트 엔진은 식별자로 해석한다.

```javascript
var person = {
  name: "Lee",
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person["name"]); // Lee

console.log(person[name]); // ReferenceError: name is not defined
```

위 예제에서 참조에러가 발생한 이유는 대괄호 연산자 내의 따옴표로 감싸지 않은 이름, 즉 **식별자 name**을 평가하기 위해 선언된 name을 찾았지만 찾지 못했기 때문이다.
객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다. 이때 ReferenceError는 발생하지 않는다.

프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않으면, 반듯기 대괄호 표기법을 사용해야 한다.
단, 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
그 외의 경우 대괄호 내에 들어가는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.

```javascript
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

위 예제에서, 브라우저 환경과 Node.js 환경의 결과가 다른 이유

- Node.js : person.last를 먼저 평가한다. person객체에는 last인 프로퍼티가 없기 때문에 person.last는 undefined로 평가된다. 즉, undefined - name이므로 참조에러
- 브라우저 : 브라우저 환경에서는 name이라는 전역 변수(전역 객체 window의 프로퍼티)가 암묵적으로 존재한다. 즉, undefined - '' 이므로 NaN

## 프로퍼티 삭제

delete 연산자는 객체의 프로퍼티를 삭제한다. 이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다. 만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.

```javascript
var person = {
  name: "Lee",
};

// 프로퍼티 동적 생성
person.age = 20;

// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;

console.log(person); // {name: "Lee"}
```

## ES6에서 추가된 객체 리터럴의 확장 기능

ES6에서는 더욱 간편하고 표현력있는 객체 리터럴의 확장 기능을 제공한다.

### 프로퍼티 축약 표현

ES5에서는 아래와 같은 표현식이 가능하다.

```javascript
// ES5
var x = 1,
  y = 2;

var obj = {
  x: x,
  y: y,
};

console.log(obj); // {x: 1, y: 2}
```

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다. **이때 프로퍼티 키는 변수 이름으로 자동 생성된다.**

```javascript
// ES6
let x = 1,
  y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

### 계산된 프로퍼티 이름

문자열 또는 문자열 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.
단 프로퍼티 키롷 사용할 표현식을 대괄호([...])로 묶어야 한다. 이를 **계산된 프로퍼티 이름**이라 한다.

ES5에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 **외부**에서 대괄호 표기법을 사용해야 한다.,

```javascript
// ES5
var prefix = "prop";
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

ES6에서는 객체 리터럴 **내부**에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있다.

```javascript
// ES6
const prefix = "prop";
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 메서드 축약 표현

ES6부터는 메서드 축약표현이 가능하다

ES5

```javascript
// ES5
var obj = {
  name: "Lee",
  sayHi: function () {
    console.log("Hi! " + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

ES6

```javascript
// ES6
const obj = {
  name: "Lee",
  // 메서드 축약 표현
  sayHi() {
    console.log("Hi! " + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```
