![](https://i.imgur.com/LOqoalW.png)

본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 타입 변환

자바스크립트의 모든 값은 타입을 가진다.

개발자가\*의도적으로 타입을 변환하는 것을 **명시적 타입 변환(타입 캐스팅)**이라 한다.

```javascript
// 명시적 타입 변환(타입 캐스팅) 예제
var x = 10;
var str = x.toString();
console.log(typeof str, str); // string 10
console.log(typeof x, x); // number 10
```

개발자의 의도와 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것을 **암묵적 타입 변환(타입 강제 변환)**이라 한다.

```javascript
// 암묵적 타입 변환(타입 강제 변환) 예제
var x = 10;
var str = x + "";
console.log(typeof str, str); // string 10

console.log(typeof x, x); // number 10
```

암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아니다.
자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 **피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다.**

### 암묵적 타입 변환은 나쁜걸까?

개발자의 의지가 담기지 않았다고 해서 암묵적 타입 변환이 나쁜 것은 아니다.
때로는 명시적 타입 변환보다 암묵적 타입 변환이 가독성 측면에서 더 좋을 수도 있다.

예로, 자바스크립트 문법을 잘 이해하고 있는 개발자에게는 (10).toString()보다 10+''이 더욱 간결하고 이해하기 쉽다.

### 암묵적 타입 변환을 더 깊이 알아보자

자바스크립트 엔지은 표현식을 평가할 때 코드의 문맥을 고려한다.

```javascript
// 문맥상 피연산자가 모두 문자열 타입이어야 한다.
"10" + 2; // -> '102'
// 문맥상 피연산자가 모두 숫자 타입이어야 한다.
5 * "10"; // -> 50
// 문맥상 피연산자 또는 표현식이 불리언 타입이어야 한다.
!0; // true
if (1) {
}
```

## 암묵적 타입 변환

### 문자열 타입으로의 변환

```javascript
1 + "2"; // -> "12"
```

위 예제의 + 연산자의 역활은 문자열 값을 만드는 것이다.
문자열 연결 연산자의 모든 피연산자는 코드의 문맥상 모두 문자열 타입이어야 한다.
즉, 자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 **모든 피연산자를 문자열 타입으로 암묵적 타입 변환한다.**

JS엔진은 연산자 표현식뿐만 아닌, 다른 표현식 또한 코드 문맥에 부합하도록 암묵적 타입 변환을 실행한다.
예로, ES6에서 도입된 템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

```javascript
"1 + 1 = ${1 + 1}"; // -> "1 + 1 = 2"
```

```javascript
// 숫자 타입
0 + '' // -> "0"
-0 + '' // -> "0"
1 + '' // -> "1"
-1 + '' // -> "-1"
NaN + '' // -> "NaN"
Infinity + '' // -> "Infinity"
-Infinity + '' // -> "Infinity"

// 불리언 타입
true + '' // -> "true"
false + '' // -> "false"

// null 타입
null + '' // "undefined"

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + '' // -> "[object Object]"
Math + '' // -> "[object Math]"
[] + '' // -> ""
[10, 20] + '' // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + '' // -> "function Array(){ [native code] }"
```

### 숫자 타입으로의 변환

```javascript
1 - "1"; // -> 0
1 * "10"; // -> 10
1 / "one"; // -> NaN
```

위 코드의 모든 연산자는 **산술 연산자**이다.
산술 연산자의 역활은 숫자 값을 만드는 것이다. 따라서 모든 피연산자는 코드 문맥상 모두 숫자 타입어야 한다.

즉, JS엔진은 산술 연산자 표현식을 평가하기 위해 모든 피연산자를 숫자 타입으로 암묵적 타입 변환한다.
이 때, 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으로 표현식의 평가 결과는 NaN이 된다.

피연산자를 수자 타입으로 변환해야 할 문맥은 산술 연산자뿐만이 아니다.

```javascript
"1" > 0; // -> true
```

또한 + 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.

```javascript
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> 0
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 1
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  symbol() + // -> TypeError: Cannot convert a symbol
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

빈 문자열, 빈 배열, null, false는 0으로 true는 1로 변환된다.
객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다.

### 불리언 타입으로 변환

```javascript
if ("") console.log(x);
```

if 문이나 for 문과 같은 제어문 또는 삼항 조건 연산자의 조건식을 불리언 값, 즉 논리적 참/거짓으로 평가되어야 한다.
JS엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다.

```javascript
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");
// 2 4
```

이때, JS엔진은 불리언 타입이 아닌 값을 Truthy 값 또는 Falsy 값으로 구분한다.
즉, 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 true로, Falsy 값은 false로 암묵적 타입 변환된다.
**false, undefined, null 0, -0, NaN, '' 등은 모두 Falsy 값이다. 이와 같은 Falsy 값 외의 모든 값은 모두 true로 평가되는 Truthy 값이다.**

```javascript
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy("");

// 모두 true를 반환한다.
isTruthy(true);
isTruthy("0"); // 빈 문자열이 아닌 문자열은 Truthy 값이다.
isTruthy({});
isTruthy([]);
```

tip) **'0'과 같이 빈 문자열이 아닌 문자열은 Truthy 값이다.**

tip) 표준 빌트인 생성자 함수와 빌트인 메서드

- 표준 빌트인 생성자 함수와 빌트인 메서드는 모두 자바스크립트에서 기본 제공하는 함수이다.
- 표준 빌트인 생성자 함수는 객체를 생성하기 위한 함수로, new 연산자와 함께 호출한다.
- 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 **빌트인 객체의 메서드**이다.

## 명시적 타입 변환

### 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출하는 방법
2. Object.prototype.toString 메서드를 사용하는 방법
3. 문자열 연결 연산자를 이용하는 방법

```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // -> "true"
String(false); // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString(); // -> "1"
NaN.toString(); // -> "NaN"
Infinity.toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
true.toString(); // -> "true"
false.toString(); // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + ""; // -> "1"
NaN + ""; // -> "NaN"
Infinity + ""; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + ""; // -> "true"
false + ""; // -> "false"
```

### 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 다음과 같다.

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. ParseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
3. +단항 산술 연산자를 이용하는 방법
4. \*산술 연산자를 이용하는 방법

```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true); // -> 1
Number(false); // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt("0"); // -> 0
parseInt("-1"); // -> -1
parseFloat("10.53"); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+"0"; // -> 0
+"-1"; // -> -1
+"10.53"; // -> 10.53
// 불리언 타입 => 숫자 타입
+true; // -> 1
+false; // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
"0" * 1; // -> 0
"-1" * 1; // -> -1
"10.53" * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1; // -> 1
false * 1; // -> 0
```

### 불리언 타입으로 변환

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean("x"); // -> true
Boolean(""); // -> false
Boolean("false"); // -> true
// 숫자 타입 => 불리언 타입
Boolean(0); // -> false
Boolean(1); // -> true
Boolean(NaN); // -> false
Boolean(Infinity); // -> true
// null 타입 => 불리언 타입
Boolean(null); // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({}); // -> true
Boolean([]); // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!"x"; // -> true
!!""; // -> false
!!"false"; // -> true
// 숫자 타입 => 불리언 타입
!!0; // -> false
!!1; // -> true
!!NaN; // -> false
!!Infinity; // -> true
// null 타입 => 불리언 타입
!!null; // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{}; // -> true
!![]; // -> true
```

# 단축 평가

### 논리 연산자를 사용한 단축 평가

**논리합(||)과 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다.**
**논리합(||)과 논리곱(&&) 연산자 표현식은 언제나 2개의 피현산자 중 어느 한쪽으로 평가된다.**

```javascript
"Cat" && "Dog"; // -> "Dog"
"Cat" || "Dog"; // -> "Cat"
```

(1)예제에서 논리곱 연산자는 좌항에서 우항으로 평가가 진행된다.
첫 번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가된다.
두 번째 피연산자가 위 논리곱 연산자 표현식의 평가 결과를 결정한다.
이때 논리곱 연산자는 논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 'Dog'를 그대로 반환한다

(2)예제에서 논리합 연산자는 첫 번째 피연산자만 가지고도 평가 결과를 결정할 수 있다.
**논리 연산의 결과를 결정한 첫 번째 피연산자, 문자열 'Cat'을 그대로 반환한다.**

예시)

```javascript
// 논리합(||) 연산자
"Cat" || "Dog"; // -> "Cat"
false || "Dog"; // -> "Dog"
"Cat" || false; // -> "Cat"

// 논리곱(&&) 연산자
"Cat" && "Dog"; // -> "Dog"
false && "Dog"; // -> false
"Cat" && false; // -> false
```

단축 평가는 다음과 같은 상황에서 유용하다.

1. 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

```javascript
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null (에러를 일으킨다)
```

```javascript
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```

2. 함수 배개변수에 기본값을 설정할 때
   함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다. 이때 단축평가를 사용해 매개변수의 기본값을 설정하면 Undefined로 인해
   발생할 수 있는 에러를 방지할 수 있다.

```javascript
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = "") {
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2
```
