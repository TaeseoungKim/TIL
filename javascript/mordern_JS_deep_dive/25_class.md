본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 클로저

## 클래스는 새로운 객체 생성 메커니즘

자바스크립트는 클래스가 필요없는 **프로토타입 기반 객체지향 언어**

ES6부터 클래스를 도입하지만, 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객체지향 모델을 제공하는 것은 아니다. 사실 클래스는 함수이며 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 **문법적 설탕**이라고 볼 수도 있다.

클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료하다. 특히 extends와 super 키워드는 상속 관계 구현을 더욱 간결하고 명료하게 한다.

생성자 함수는 가지고 있지 않은 **클래스가 가진 특징**

- new 연산자 없이 호출하면 에러가 발생
- 상속을 지원하는 extends와 super 키워드를 제공
- 클래스는 호이스팅이 발생하지 않는 것 처럼 동작
- 클래스 내의 모든 코드는 암묵적 strict mode로 지정되고, 해제할 수 없음
- 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어티리뷰트 [[Enumeralble]]의 값이 false이다. 즉, 열거 불가

클래스는 가지고 있지 않은 **생성자 함수가 가진 특징**

- 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출
- 함수 선언문으로 선언한 생성자 함수는 함수 호이스팅, 함수 표현식으로 선언한 생성자 함수는 변수 호이스팅

## 클래스 정의

- class 키워드를 사용하여 정의
- 클래스 이름은 주로 파스칼 케이스를 사용
- 클래스는 일급 객체
- 무명의 리터럴로 생성 가능. 즉, 런타임에 생성이 가능
- 변수나 자료구조(객체, 배열 등)에 저장 가능
- 함수의 매개변수에게 전달 가능
- 함수의 반환값으로 사용 가능

```javascript
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

```

```

```

```

## 클래스 호이스팅

클래스가 평가되어 **생성된 함수 객체는 constructor(생성자 함수로서 호출할 수 있는 함수)이다.**
생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
즉, 런타임 이전(소스코드 평가과정)에 먼저 평가되어 함수 객체를 생성한다.

```javascript
const Person = "";

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```

위 예제를 보자.

클래스는 함수로 평가된다.
변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생하지만, **호이스팅이 발생하지 않는 것처럼 동작한다.**
**let, const 키워드로 선언한 변수처럼 호이스팅 되어 선언과 초기화가 분리되어 진행**되기 때문이다.

TDZ(일시적 사각지대)에 빠지게 되어 호이스팅이 발생하지 않는 것처럼 동작한다.

## 인스턴스 생성

클래스는 생성자 함수이며, 인스턴스 생성 시에 **반드시 new 연산자와 함께 호출해야 한다.**
클래스 표현식으로 정의된 클래스 경우, 클래스를 가리키는 식별자를 사용해 인스턴스를 생성한다.
**클래스 이름은 함수와 동일하게 클래스 몸체 내부에서만 유효**한 식별자다.

```javascript
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new Person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
console.log(MyClass); // ReferenceError: MyClass is not defined

const you = new MyClass(); // ReferenceError: MyClass is not defined 5. 메서드
```

클래스 몸체에서 정의할 수 있는 메서드는 세 가지가 있다.

- constructor
- 프로토타입 메서드
- 정적 메서드

### constructor

- 인스턴스를 생성하고 초기화한다.
- 클래스 내에 최대 한개만 존재
- constructor는 생략 가능(생략하면 빈 객체를 생성)
- 별도의 반환문을 갖지 않아야 함
- return을 사용 안하는 것이 바람직함
- this가 아닌 다른 객체를 명시적으로 return하면 해당 객체가 반환
- 원시값을 return하면 무시하고 암묵적으로 this 반환
- 함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프 체인을 구성한다.

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

### 프로토타입 메서드

생성자 함수와 다르게 클래스는 프로토타입 메서드를 사용하지 않아도 기본적으로 프로토타입 메서드 타입이 된다.

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 됨
프로토타입 메서드는 프로토타입 객체로 바인딩
인스턴스는 프로토타입 메서드를 상속받아 사용 가능

```javascript
// me 객체의 프로토타입은 Person.prototype이다.
Object.getPrototypeOf(me) === Person.prototype; // -> true
me instanceof Person; // -> true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
me instanceof Object; // -> true

// me 객체의 constructor는 Person 클래스다.
me.constructor === Person; // -> true
```

### 정적(static) 메서드

인스턴스를 생성하지 않아도 호출할 수 있는 메서드
static 키워드를 붙여서 생성한다.

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}
```

- **클래스에 바인딩**
- 정적 메서드는 인스턴스로 호출할수 없고 **클래스로 호출**
- 정적 메서드가 바인딩 된 클래스는 인스턴스의 프로토타입 체인상에 존재하지 않기 때문

### 정적 메서드와 프로토타입 메서드의 차이

- **자신이 속해 있는 프로토타입 체인이 다르다**
- **정적 메서드는 클래스로 호출**하고, **프로토타입 메서드는 인스턴스로 호출**한다.
- 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만, 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있음
- **this 바인딩이 다름**

---

### 클래스에서 정의한 메서드의 특징

function 키워드를 생략한 메서드 축약 표현을 사용
메서드를 정의할 때는 콤마가 필요 없음
암묵적으로 strict mode 실행
for ... in 문이나 Object.keys 메서드 등으로 열거 불가
프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false
내부 메서드 [[Construct]]를 갖지 않는 non-constructor -> new연산자와 함께 호출할 수 없음

### 클래스의 인스턴스 생성 과정

클래스의 인스턴스는 new 연산자와 함께 호출
new 연산자 없이는 호출 불가
new 연산자와 클래스 호출 -> 클래스의 내부 메서드 [[Construct]]가 호출
(생성자 함수와 동일)
아래 과정을 통해 인스턴스 생성

1. 인스턴스 생성과 this 바인딩
   new 연산자로 클래스를 호출 -> 암묵적으로 빈 객체 생성
   생성된 인스턴스의 프로토타입은 클래스의 prototype 프로퍼티가 가리키는 객체로 설정됨
   암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩
2. 인스턴스 초기화
   this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가
   constructor가 전달받은 초기값 인수로 인스턴스의 프로퍼티 값을 초기화
3. 인스턴스 반환
   완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

### 프로퍼티

5가지 프로퍼티 종류가 있음.
아직 ECMAScript의 표준사양은 1), 2) 만 해당
ES2022(ES13)부터 3), 4), 5) 모두 ECMAScript의 표준사양

1. 인스턴스 프로퍼티
   constructor 내부에서 정의해야 함
   constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 됨
   인스턴스 프로퍼티는 언제나 public

```javscript
class Person {
constructor(name) {
// 인스턴스 프로퍼티
this.name = name; // name 프로퍼티는 public하다.
}
}

const me = new Person("Lee");

// name은 public하다.
console.log(me.name); // Lee
```

2. 접근자 프로퍼티
   자체적으로는 값([[Value]])을 갖지 않음
   다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수 getter, setter 로 구성
   getter 는 반드시 무언가를 반환
   setter는 반드시 매개변수가 있어야 함
   setter는 단 하나의 값만 할당받기 때문에 단 하나의 매개변수만 선언할 수 있음
   클래스의 메서드는 기본적으로 프로토타입 메서드 -> 클래스의 접근자 프로퍼티는 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티

```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  }
}

const me = new Person("Ungmo", "Lee");

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
me.fullName = "Heegun Lee";
console.log(me); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

3. 클래스 필드 정의
   클래스필드
   클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어
   자바에서는 마치 클래스 내부에서 변수처럼 사용
   기존 자바스크립트의 클래스 몸체에는 메서드만 선언 가능 -> 자바와 유사하게 클래스 필드를 선언하면 문법 에러가 발생
   클래스 몸체에서 클래스 필드를 정의할 수 있는 것
   인스턴스 프로퍼티를 마치 클래스 기반 객체 지향 언어의 클래스 필드처럼 정의 할 수 있는 새로운 표준사양
   최신브라우저(Chrome 72이상) 또는 Node.js(버전12이상)에는 동작
   모든 클래스 필드는 인스턴스 프로퍼티

```javascript
class Person {
  // 클래스 필드 정의
  name = "Lee";
}

const me = new Person();
console.log(me); // Person {name: "Lee"}
```

Class field declarations 로 인해 인스턴스 프로퍼티를 정의하는 방식은 두 가지가 됨

인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화해야 한다면 -> 기존의 constuctor에서 인스턴스 프로퍼티를 정의하는 방식
외부 초기값으로 클래스 필드를 초기화할 필요가 없다면 -> 기존 방식 or 클래스 필드 정의 제안 방식
특징

this에 클래스 필드를 바인딩해서는 안됨(this는 constructor와 메서드 내에만 사용 가능)

```javascript
class Person {
// this에 클래스 필드를 바인딩해서는 안된다.
this.name = ''; // SyntaxError: Unexpected token '.'
}
클래스 필드를 참조하는 경우, this를 반드시 사용해야 함
class Person {
// 클래스 필드
name = "Lee";

constructor() {
console.log(name); // ReferenceError: name is not defined
}
}

new Person();
클래스 필드에 초기값을 할당하지 않은 겨우 undefined
class Person {
// 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
name;
}

const me = new Person();
console.log(me); // Person {name: undefined}
함수를 클래스 필드에 할당 가능 -> 클래스 필드를 통해 메서드를 정의 가능
함수는 프로토타입 메서드가 아닌, 인스턴스 메서드가 됨 -> 클래스 필드에 함수를 할당하는 것은 권장되지 않음
class Person {
// 클래스 필드에 문자열을 할당
name = "Lee";

// 클래스 필드에 함수를 할당
getName = function () {
return this.name;
};
// 화살표 함수로 정의할 수도 있다.
// getName = () => this.name;
}

const me = new Person();
console.log(me); // Person {name: "Lee", getName: ƒ}
console.log(me.getName()); // Lee
```

4. private 필드
   private 필드의 앞에 #을 붙임
   참조 할 때도 앞에 #을 붙임
   내부에서만 참조 가능

```javascript
class Person {
// private 필드 정의
#name = "";

constructor(name) {
// private 필드 참조
this.#name = name;
}
}

const me = new Person("Lee");

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
private 필드는 반드시 클래스 몸체에 정의
class Person {
constructor(name) {
// private 필드는 클래스 몸체에서 정의해야 한다.
this.#name = name;
// SyntaxError: Private field '#name' must be declared in an enclosing class
}
}
```

접근 가능성 public private
클래스 내부 O O
자식 클래스 내부 O X
클래스 인스턴스를 통한 접근 O X 5) static 필드
static public 필드, static private 필드, static private 메서드를 정의
-> 인스턴스를 생성하지 않고 메서드 사용 가능

```javascript
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

## 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

기존 클래스를 상속받아 새로운 클래스를 확장하여 정의
클래스와 생성자 함수는 인스턴스를 생성할 수 있는 함수라는 점에서 매우 유사
클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드가 기본적으로 제공

```javascript
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() {
    return "eat";
  }

  move() {
    return "move";
  }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() {
    return "fly";
  }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

### extends 키워드

extends 키워드를 사용하여 상속받은 클래스를 정의
서브 클래스(파생 클래스, 자식 클래스): 상속을 통해 확장된 클래스
수퍼 클래스(베이스 클래스, 부모 클래스): 서브클래스에게 상속된 클래스
// 수퍼(베이스/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}

### 동적 상속

extends 키워드를 통해 생성자 함수를 상속 가능

```javascript
// 생성자 함수
function Base(a) {
  this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```

extends 키워드 다음에는 클래스, [[Constructor]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용 가능
이를 통해 동적을 상속받을 대상을 결정 가능

```javascript
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### 서브 클래스의 constructor

클래스에서 constructor를 생략하면 암묵적으로 빈 객체가 정의된다

```javascript
// 수퍼클래스
class Base {
constructor() {}
}

// 서브클래스
class Derived extends Base {
constructor() {
super();
}
}

const derived = new Derived();
console.log(derived); // Derived {}
서브 클래스에 constructor를 생략하면 암묵적으로 super(...args)가 정의 됨
constructor(...args) { super(...args); }
// 수퍼클래스
class Base {
constructor(a, b) {
this.a = a;
this.b = b;
}
}

// 서브클래스
class Derived extends Base {
// 다음과 같이 암묵적으로 constructor가 정의된다.
// constructor(...args) { super(...args); }
}

const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}
```

super()는 수퍼클래스의 constructor를 호출하여 인스턴스를 생성

### super 키워드

함수처럼 호출할 수도 있고, this와 같이 참조할 수 있는 특수한 키워드
super 호출 -> 수퍼 클래스의 constructor(super-constructor)를 호출

```javascript
// 수퍼클래스
class Base {
  constructor(a, b) {
    // ④
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  constructor(a, b, c) {
    // ②
    super(a, b); // ③
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3); // ①
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```

super 호출 주의 사항

서브클래스에서 constructor을 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야 함

```javascript
class Base {}

class Derived extends Base {
constructor() {
// ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
console.log("constructor call");
}
}

const derived = new Derived();
서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없음
class Base {}

class Derived extends Base {
constructor() {
// ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
this.a = 1;
super();
}
}

const derived = new Derived(1);
```

서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생 -> super는 반드시 서브클래스의 constructor에서만 호출

```javascript
class Base {
constructor() {
super(); // SyntaxError: 'super' keyword unexpected here
}
}

function Foo() {
super(); // SyntaxError: 'super' keyword unexpected here
}
super 참조 -> 수퍼클래스의 메서드를 호출 할 수 있음

// 수퍼클래스
class Base {
constructor(name) {
this.name = name;
}

sayHi() {
return `Hi! ${this.name}`;
}
}

// 서브클래스
class Derived extends Base {
sayHi() {
// super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
return `${super.sayHi()}. how are you doing?`;
}
}

const derived = new Derived("Lee");
console.log(derived.sayHi()); // Hi! Lee. how are you doing?
// 수퍼클래스
class Base {
constructor(name) {
this.name = name;
}

sayHi() {
return `Hi! ${this.name}`;
}
}

class Derived extends Base {
sayHi() {
// **super는 Base.prototype을 가리킨다.
const **super = Object.getPrototypeOf(Derived.prototype);
return `${__super.sayHi.call(this)} how are you doing?`;
}
}
```

[[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
[[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.

예를 들어, Derived 클래스의 sayHi 메서드는 Derived.prototype에 바인딩되어 있다.
따라서 Derived 클래스의 sayHi 메서드의 [[HomeObject]]는 Derived.prototype이고
이를 통해 Derived 클래스의 sayHi 메서드 내부의 super 참조가 Base.prototype으로 결정된다.
따라서 super.sayHi는 Base.prototype.sayHi를 가리키게 된다.

```javascript
super = Object.getPrototypeOf([[HomeObject]])
```

### 상속 클래스의 인스턴스 생성 과정

(1) 서브클래스의 super 호출
(2) 수퍼클래스의 인스턴스 생성과 this 바인딩
(3) 수퍼클래스의 인스턴스 초기화
(4) 서브클래스 constructor로의 복귀와 this 바인딩
(5) 서브클래스의 인스턴스 초기화
(6) 인스턴스 반환

```javascript
// 수퍼클래스
class Rectangle {
constructor(width, height) {
this.width = width;
this.height = height;
}

getArea() {
return this.width \* this.height;
}

toString() {
return `width = ${this.width}, height = ${this.height}`;
}
}

// 서브클래스
class ColorRectangle extends Rectangle {
constructor(width, height, color) {
super(width, height);
this.color = color;
}

// 메서드 오버라이딩
toString() {
return super.toString() + `, color = ${this.color}`;
}
}

const colorRectangle = new ColorRectangle(2, 4, "red");
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 메서드를 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red 7. 표준 빌트인 생성자 함수 확장
```

표준 빌트인 객체는 [[Construct]] 내부 메서드를 갖는 생성자 함수 -> extends 키워드를 통해 확장 가능

```javascript
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75
```
