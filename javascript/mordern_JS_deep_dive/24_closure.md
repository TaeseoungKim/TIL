본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 클로저

클로저는 자바스크립트 고유의 개념이 아니다. 함수를 일급 객체로 취급하는 **함수형 프로그래밍 언어**에서 사용되는 중요한 특성이다.
**외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.
이러한 중첩 함수를 클로저라고 부른다.**
자세한 내용은 뒤에서 설명하겠다.

## 렉시컬 스코프

**자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.**
이를 렉시컬 스코프(정적 스코프)라고 한다.

렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.
이것이 바로 렉시컬 스코프다.

```javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

## 함수 객체의 내부 슬롯 [[Environment]]

렉시컬 스코프가 가능하려면 자신이 호출되는 환경과는 상관없이 자신이 정의된 환경, 즉 상위 스코프(함수 정의가 위치하는 스코프)를 기억해야 한다.
이를 위해 함수는 자신의 내부 슬롯 [[Environment]]에 자신의 상퀴 스코프에 대한 참조를 저장한다.

- 전역에서 정의된 함수 선언문은 전역 코드가 평가되는 시점에 평가되어 함수 객체를 생성한다. 즉, [[Environment]]에는 전역 렉시컬 환경의 참조가 저장된다.
- 함수 내부에서 정의된 함수 선언문은 외부 함수 코드가 평가되는 시점에 평가되어 함수 객체를 생성한다. 즉, [[Environment]]에는 외부 렉시컬 환경의 참조가 저장된다.

## 클로저와 렉시컬 환경

```javascript
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

위 예제를 보면 outer 함수가 종료되면 outer 함수는 생명 주기를 마감하게 되어 지역 변수 x는 더이상 유효하지 않다.
하지만 실행결과 (4)를 보면 지역 변수 x가 다시 부활이라도 한 듯이 동작하고 있다.

**이처럼 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.
이러한 중첩 함수를 클로저라고 부른다.**

**중요한 것은, "outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다."**
-> outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다. 가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제하지 않는다.

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다.
하지만 일반적으로 모든 함수를 클로저라고 하지는 않는다. **상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아니다.**
**상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다.**

```javascript
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;

      // 일반적으로 클로저라고 하지 않는다.
      // bar 함수는 클로저였지만 곧바로 소멸한다.
      function bar() {
        debugger;
        // 상위 스코프의 식별자를 참조한다.
        console.log(x);
      }
      bar();
    }

    foo();
  </script>
</body>
</html>
```

위 예제를 보면 외부 함수 foo보다 중첩 함수 bar의 생명 주기가 짧다. 이런 경우 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않는다.

```javascript
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 클로저
      // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
      function bar() {
        debugger;
        console.log(x);
      }
      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```

위 예제처럼, 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.
다만 위 예제에서, bar는 foo의 x만 기억하고 있다. 대부분의 모던 브라우저는 최적화를 통해 상위 스코프의 식별자 중에서 클로저가 참조하고 있는 식별자(x)만을 기억한다.
상위 스코프의 식별자 중에서 클로저가 참조하고 있는 식별자를 **자유 변수**라고 부른다.
클로저란 **함수가 자유 변수에 대해 닫혀있다**라는 의미다. 즉, **자유 변수에 묶여있는 함수**라고 할 수 있다.

## 클로저의 활용

클로저는 **상태를 안전하게 변경하고 유지하기 위해 사용한다. 즉, 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.**

```javascript
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가 시킨다.
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 예제는 잘 동작하지만 아래와 같은 문제점을 지니고 있다.

1. 카운트 상태는 increase 함수가 호출되기 전까지 변경되지 않고 유지 되어야 한다.
2. 이를 위해 카운트 상태는 increase 함수만이 변경할 수 있어야 한다.

하지만 카운트 상태는 전역 변수이기 때문에 언제든지 누구나 접근/변경이 가능하다.
이로 인해, 의도치 않은 변경으로 오류가 생길 수 있다.

```javascript
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저
  return function () {
    // 카운트 상태를 1만큼 증가 시킨다.
    return ++num;
  };
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 코드가 실행되면 즉시 실행 함수가 호출되고, 즉시 실행 함수가 반환한 함수가 increase 변수에 할당된다.
increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.

즉시 실행 함수는 종료됐지만, 클로저는 increase 변수에 할당되어 호출된다.
이 때, 클로저는 상위 스코프의 num을 참조하고 있기 때문에 카운트 상태를 유지할 수 있다.

이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

```javascript
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저인 메서드를 갖는 객체를 반환한다.
  // 객체 리터럴은 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
  return {
    // num: 0, // 프로퍼티는 public하므로 은닉되지 않는다.
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    },
  };
})();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

위 예제에서 즉시 실행 함수가 반환하는 객체 리터럴은 즉시 실행 함수의 실행 단계에서 평가되어 객체가 된다.
이때 객체의 메서드도 함수 객체로 생성된다. 객체 리터럴의 중괄호는 코드 블록이 아니므로 별도의 스코프를 생성하지 않는다.

위 예제의 increase, decrease 메서드의 상위 스코프는 increase, decrease 메서드가 평가되는 시점에 실행중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경이다.
따라서 increase, decrease가 언제 어디서 호출되든 상관없이 increase, decrease 함수는 즉시 실행 함수의 스코프의 식별자를 참조할 수 있다.

**Q) 이것도 클로저인가요? 중첩 함수가 아닌데**

```javascript
const Counter = (function () {
  // ① 카운트 상태 변수
  let num = 0;

  function Counter() {
    // this.num = 0; // ② 프로퍼티는 public하므로 은닉되지 않는다.
  }

  Counter.prototype.increase = function () {
    return ++num;
  };

  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0;
  };

  return Counter;
})();

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

생성자 함수 Counter는 프로토타입을 통해 increase, decrease 메서드를 상속받는 인스턴스를 생성한다.
increase, decrease 메서드는 모두 자신의 함수 정의가 평가되어 함수 객체가 될 때 실행 중인 컨텍스트인 즉시 실행 함수 실행 컨텍스트 렉시컬 환경을 기억하는 클로저다.
따라서 프로토타입을 통해 상속되는 프로토타입 메서드일지라도 즉시 실행 함수의 자유 변수 num을 참조할 수 있다.

```javascript
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase); // ①
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // ②
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

**위 예제를 보면 increaser, decreaser는 자유 변수 n을 공유하지 않는다.**
increaser, decreaser에 할당된 함수는 **각각 자신만의 독립된 렉시컬 환경을 갖기 때문이다.**

따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다.

```javascript
// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 함수를 인수로 전달받는 클로저를 반환
  return function (aux) {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
})();

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

## 캡슐화와 정보 은닉

캡슐화: 객체의 상태를 나타내는 프로퍼티와 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것
