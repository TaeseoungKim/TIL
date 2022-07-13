본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 프로토타입

C++나 자바 같은 클래스 기반 객체지향 프로그래밍 언어의 특징인 클래스와 상속, 캡슐화를 위한 키워드인 public, privated, protected 등이 없어서
자바스크립트는 객체지향 언어가 아니라고 오해하는 경우도 있다.
하지만 자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.
자바스크립트는 클래스 기반 객체지향 언어보다, 효율적이고 강력하다.

**자바스크립트를 이루고 있는 원시 값을 제외한 거의 "모든 것"이 객체**다.

## 객체지향 프로그래밍

객체지향 프로그래밍은 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 **여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현**하려는 프로그래밍 패러다임을 말한다.

객체지향 프로그래밍은 객체의 **상태**를 나타내는 데이터와 상태 데이터를 조작할 수 있는 **동작**으로 구성된다.

- **추상화**: 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것

## 상속과 프로토타입

상속: OOP의 핵심 개념, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용하는 것

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

위 예제를 보면 문제점이 있다.
바로 circle1, circle2 모두 getArea 메서드를 중복 생성하여 가지고 있는 것이다. (메모리 낭비)
인스턴스를 생성할 때 마다 메서드를 생성하므로 퍼포먼스에 악영향을 준다. 만약 10개의 인스턴스를 생성하면 내용이 동일한 메서드도 10개 생성된다.

위 예제를 아래와 같이 고쳐보자.

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

Circle 생성자 함수가 생성한 모든 인스턴스는 **부모 객체의 역할을 하는 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.**
**즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.**

상속은 코드의 재사용이란 관점에서 매우 유용하다.
생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현 없이 상위(부모) 객체인 프로토타입의 자산을 공유하여 사용할 수 있다.

## 프로토타입 객체

프토토타입 객체

- 객체 간 상속을 구현하기 위해 사용된다. 어떤 객체의 상위(부모) 역활을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다.
- 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 상속할 수 있다.
- **모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조(null인 경우도 있다)다.**
- [[Prototype]]의 값은 객체 생성 방식에 의해 결정된다. 모든 객체는 하나의 프로토타입을 가진다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.
- [[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만, \_\_prototype\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있다.
- 그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

### \_\_proto\_\_ 접근자 프로퍼티

아래 예제를 크롬 브라우저의 콘솔에서 출력해보자.

```javascript
const person = { name: "Lee" };
undefined;
person;
```

그럼 아래와 같은 값이 출력된다.
'[[Prototype]]: Object' 아래부터가 person 객체의 프로토타입인 Object.prototype이다.
이는 \_\_proto\_\_ 접근자 프로퍼티를 통해 person 객체의 [[prototype]] 내부 슬롯이 가리키는 객체인 Object.prototype에 접근한 결과를 크롬 부라우저가 콘솔에 표시한 것이다.
이처럼 모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입을 가리키는 [[prototype]] 내부 슬롯에 접근할 수 있다.

```javascript
{name: 'Lee'}
name: "Lee"
[[Prototype]]: Object
constructor: ƒ Object()
hasOwnProperty: ƒ hasOwnProperty()
isPrototypeOf: ƒ isPrototypeOf()
propertyIsEnumerable: ƒ propertyIsEnumerable()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
valueOf: ƒ valueOf()
__defineGetter__: ƒ __defineGetter__()
__defineSetter__: ƒ __defineSetter__()
__lookupGetter__: ƒ __lookupGetter__()
__lookupSetter__: ƒ __lookupSetter__()
__proto__: (...)
get __proto__: ƒ __proto__()
set __proto__: ƒ __proto__()
```

#### **\_\_proto\_\_**는 접근자 프로퍼티다.

내부 슬롯은 프로퍼티가 아니다. 따라서 자바스크립트는 **원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않는다.**
단, 간접적으로 접근할 수 있는 수단을 제공하는데 이것이 **접근자 프로퍼티**다.
즉, \_\_proto\_\_를 통해 [[prototype]] 내부 슬롯에 접근할 수 있다.

- \_\_proto\_\_ 호출하면, get \_\_proto\_\_
- \_\_proto\_\_에 새로운 프로토타입을 할당하면, set \_\_proto\_\_ 호출

```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

#### \_\_proto\_\_는 상속을 통해 사용된다.

\_\_proto\_\_ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
모든 객체는 상속을 통해 Object.prototype.\_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있다.

```javascript
const person = { name: "Lee" };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty("__proto__")); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

tip) Object.prototype
**모든 객체는 프로토타입의 계층 구조인 프로토타입 체인에 묶여 있다.**
JS엔진은 객체의 프로퍼티에 접근하려고 할 때 객체에 접근하려는 프로퍼티가 없다면, \_\_proto\_\_ 접근자 프로퍼티가 가리키는 참조를 따라 자신의 부모 역활을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 프로토타입 체인의 종점. 즉 **프로토타입 체인의 최상위 객체는 Object.prototype**이며, 이 객체의 프로퍼티와 메서드는 모든 객체에 상속된다.

#### \_\_proto\_\_를 통해 프로포타입에 접근하는 이유

[[prototype]] 내부 슬롯의 값, 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```javascript
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

**프로토타입은 단방향 링크드 리스트로 구현되어야 한다.**
위와 같은 코드가 에러없이 정상 실행된다면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인, 다시 말해 **프로토타입 체인 종점이 존재하지 않기 때문에 무한 루프에 빠진다.**
\_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

#### \_\_proto\_\_를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

아래 예제와 같이 Object.prototype를 상속받지 않는 객체를 생성할 수 있다.
이처럼 모든 객체가 \_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문에 \_\_proto\_\_를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

```javascript
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

따라서 \_\_proto\_\_ 대신, 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf 메서드를 사용하고, 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.

```javascript
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

Object.getPrototypeOf는 get Object.Prototype.\_\_proto\_\_와 정확히 일치한다.
Object.setPrototypeOf는 set Object.Prototype.\_\_proto\_\_와 정확히 일치한다.

### 함수 객체의 prototype 프로퍼티

**함수 객체는 prototype 프로퍼티를 소유한다!**

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}.hasOwnProperty("prototype")); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}.hasOwnProperty("prototype")); // -> false
```

tip)
헷갈릴 수 있다.
함수 객체는 prototype 프로퍼티를 소유하고, 일반 객체는 \_\_proto\_\_ 접근자 프로퍼티를 소유한다. 하지만 결국 두 가지 모두 동일한 프로토타입을 가리킨다.
맞나요? ㅋㅋ

위 예제와 같이 함수 객체만이 소유하는 prototype 프로퍼티는 **생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**
**하지만 생성자 함수로서 호출할 수 없는 함수, non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.**

```javascript
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {},
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

| 구분                            | 소유        | 값                | 사용 주체   | 사용 목적                                                                    |
| ------------------------------- | ----------- | ----------------- | ----------- | ---------------------------------------------------------------------------- |
| \_\_proto\_\_ (접근자 프로퍼티) | 모든 객체   | 프로토타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용                      |
| prototype (프로퍼티)            | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

### 프로토타입의 constructor 프로퍼티와 생성자 함수

**모든 프로토타입은 constructor 프로퍼티를 갖는다.**
이 **constructor 프로퍼티는 자신을 참조하고 있는 생성자 함수를 가리킨다.**
이 연결은 함수가 생성될 떄, 즉 함수 객체가 생성될 때 이뤄진다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

me 객체에는 constructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototype에는 constructor 프로퍼티가 있다.
따라서 me 객체는 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있다.

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

앞에서 살펴본 것처럼 생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.
**리터럴 표기법으로 생성하는 것처럼 생성자 함수를 호출하지 않는 객체 생성 방식은 어떨까?**

tip) 리터럴 표기법

```javascript
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
  return a + b;
};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexp = /is/gi;
```

```javascript
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

아래 예제의 2처럼 Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로는 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

```javascript
// 1. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 2. Object 생성자 함수에 의한 객체 생성
// Object 생성자 함수는 new 연산자와 함께 호출하지 않아도 new 연산자와 함께 호출한 것과 동일하게 동작한다.
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Object();
console.log(obj); // {}
```

**객체 리터럴이 평가될 때는 OrdinaryObjectCreate를를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.**
리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토 타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다.
프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다.
즉, 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다

**마지막으로 요약해보면, 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성자 함수로 생각해도 크게 무리는 없다.**
**이처럼 리터럴 표기법에 의해 생성된 객체도 몰론 프로토타입이 존재하지만, 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.**

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| ------------------ | ----------- | ------------------ |
| 객체 리터럴        | Object      | Object.prototype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.prototype   |

~~똑같쥬?~~

## 프로토타입의 생성 시점

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**

생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

```javascript
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.

```javascript
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

함수 선언문은 런타임 이전에 JS엔진에 의해 먼저 실행된다(호이스팅). 따라서 **함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다. 이때 프로토타입도 더불어 생성된다.** 생성된 프로토타입은 Person 생성자 함수의 prototype 프로퍼티에 바인딩된다.
또한, 생성된 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체다. 생성된 프로토타입의 프로토타입은 Object.prototype이다.

## 객체 생성 방식과 프로토 타입의 결정

객체는 다음과 같이 다양한 생성 방법이 있다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

위 방법 모두 OrdinaryObjectCreate에 의해 생성된다.
OrdinaryObjectCreate는 자신이 생성할 객체의 프로토타입을 인수로 전달받는다. 또 프로퍼티 목록을 옵션으로 전달할 수 있다.

OrdinaryObjectCreate 동작 방법

1. 빈 객체 생성
2. 프로퍼티 목록의 프로퍼티를 객체에 추가
3. 인수로 전달받은 프로토타입을 자신이 생성한 객체의 [[Prototype]] 내부 슬롯에 할당
4. 생성한 객체 반환

### 객체 리터럴에 의해 생성된 객체의 프로토타입

객체 리터럴에 의해 생성된 객체

1. Object.prototype을 프로토타입으로 가진다.
2. 생성자 함수를 갖지 않지만 Object.prototype의 constructor를 사용할 수 있다.

### Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수에 의해 생성된 객체

1. Object.prototype을 프로토타입으로 가진다.
2. 생성자 함수를 갖지 않지만 Object.prototype의 constructor를 사용할 수 있다.

### 생성자 함수에 의해 생성된 객체의 프로토타입

생성자 함수에 의해 생성된 객체

1. 생성된 객체의 프로토타입은 생성자 함수의 prototype에 바인딩되어 있다.

## 프로토타입 체인

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty("name")); // true
```

위 예제를 보면 me 객체의 프로토타입은 Person.prototype 이다.
Person.prototype의 프로토타입은 Object.prototype 이다.

즉, me 객체는 Person.prototype과 Object.prototype를 상속받았다.

**자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역활을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다.**

프로로타입 체인 종점에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다. 이때 에러가 발생하지 않는 것에 주의하자!

```javascript
console.log(me.foo); // undefined
```

이에 반해, 프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다.
즉, JS 엔진은 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다.

## 오버라이딩과 프로퍼티 섀도잉

오버라이딩: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식
오버로딩: 함수의 이름은 동일하지만 매개변수의 타입 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식. 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.

프로토타입 프로퍼타: 프로토타입이 소유한 프로퍼티
인스턴스 프로퍼티: 인스턴스가 소유한 프로퍼티

```javascript
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee");

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

위 예제를 보면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다.
이떄 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했다. (프로토타입 메서드 sayHello는 가려진다. = 섀도잉)

```javascript
// 인스턴스 메서드를 삭제한다.
delete me.sayHello;
// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee

// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

위 예제와 같이 delete를 통해 sayHello를 삭제해보자.
첫번째 삭제: 인스턴스 메서드가 삭제된다.
두번쨰 삭제: 하위 객체를 통해 프로토타입 프로퍼티를 변경 또는 삭제하는 것은 불가능하다. 다시 말해 get은 가능하나, set은 불가능하다.

프로토타입의 프로퍼티를 변경 또는 삭제하려면 프로토타입에 직접 접근해야한다.

```javascript
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
me.sayHello(); // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```

## 프로토타입 교체

프로토타입은 임의의 다른 객체로 변경할 수 있다. 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다.

## instanceof 연산자

instanceof 연산자는 이항 연산자로서 좌변에 객체, 우변에 생성자 함수를 피연산자로 받는다.
만약 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생한다.

**우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true 아니면 false**

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문에 false로 평가된다.
console.log(me instanceof Person); // false

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

## 직접 상속

### Object.create에 의한 직접 상속

Object.create의 첫 번째 매개변수에는 생성할 객체의 프로토타입을 지정, 두 번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체 지정(생략가능)

```javascript
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일하다.
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
// 위 코드는 다음과 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

### 객체 리터럴 내부에서 \_\_proto\_\_에 의한 직접 상속

ES6에서는 개체 리터럴 내부에서 \_\_proto\_\_ 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```javascript
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

### 정적 프로퍼티 / 메서드

**정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.**

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

이와 같이 사용할 수 있는 이유는 **생성자 함수도 객체이기에 자신의 프로퍼티/메서드를 소유**할 수 있기 때문이다.
생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼타/메서드라고 한다.

**정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.**
=> 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니기 때문이다.

정적 메서드
Object.함수이름()

프로토타입 메서드
Object.prototype.함수이름()

tip)
프로토타입 프로퍼티/메서드를 표기할 prototype을 #으로 표기하는 경우도 있으니 알아두자.
ex: Object.prototype.isPrototypeOf를 Object#isPrototypeOf으로 표기

## 프로토타입 존재 확인

```javascript
key in Object;
```

in 연사자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.
주의할 점은 확인 대상 객체의 프로퍼티 뿐만 아니라 **확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.**

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재한다.
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재한다.
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log("age" in person); // false
console.log("toString" in person); // true
```

tip)
ES6에서는 in 연산자 대신 Reflect.has 메서드를 사용할 수 있다. (동일하게 동작)

### Object.prototype.hasOwnProperty 메서드

Object.prototype.hasOwnProperty 메서드도 마찬가지로 객체에 특정 프로퍼티가 존재하는지 사용할 수 있다.
하지만 상속받은 프로토타입의 프로퍼티는 검사하지 않는다.

```javascript
const person = { name: "Lee" };

console.log(person.hasOwnProperty("name")); // true
console.log(person.hasOwnProperty("age")); // false
console.log(person.hasOwnProperty("toString")); // false
```

## 프로퍼티 열거

### for ... in 문

```javascript
for (변수선언문 in 객체) { ... }
```

객체의 모든 프로퍼티를 순회하며 열거하려면 for ... in 문을 사용한다.

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
};

// for...in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당된다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

for ... in 문은 객체의 프로퍼티 개수만큼 순회한다.
또한 상속받은 프로토타입의 프로토타입의 프로퍼티까지 열거한다.
=> toString과 같이 [[Enumerable]]의 값이 false인 경우 열거되지 않는다.
=> 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
=> 프로퍼티를 열거할 때 정의된 순서를 보장하지 않는다. (하지만 대부분의 브라우저는 정의된 순서를 보장, 숫자는 정렬)

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
  __proto__: { age: 20 },
};

for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
// age: 20

const sym = Symbol();
const obj = {
  a: 1,
  [sym]: 10,
};

for (const key in obj) {
  console.log(key + ": " + obj[key]);
}
// a: 1
```

```javascript
const arr = [1, 2, 3];
arr.x = 10; // 배열도 객체이므로 프로퍼티를 가질 수 있다.

for (const i in arr) {
  // 프로퍼티 x도 출력된다.
  console.log(arr[i]); // 1 2 3 10
}

// arr.length는 3이다.
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1 2 3
}

// forEach 메서드는 요소가 아닌 프로퍼티는 제외한다.
arr.forEach((v) => console.log(v)); // 1 2 3

// for...of는 변수 선언문에서 선언한 변수에 키가 아닌 값을 할당한다.
for (const value of arr) {
  console.log(value); // 1 2 3
}
```

### Object.key / values / entries

앞서 봤듯이, for ... in 문은 자신의 프로토타입의 프로퍼티까지 열거한다.
객체 자신의 프로퍼티만 열거하고 싶다면 **Object.key / values / entries**등의 메서드를 사용한다.

Object.key: 자신의 열거가능한 프로퍼티 키를 배열로 반환
Object.values: 자신의 열거가능한 프로퍼티 값을 배열로 반환
Object.entries: 객체 자신의 열거 가능한 키와 값의 쌍의 배열을 배열에 담아 반환
