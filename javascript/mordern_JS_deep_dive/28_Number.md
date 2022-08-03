![](https://i.imgur.com/LOqoalW.png)

본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 표준 빌트인 객체 Number

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

- 생성자 함수의 인수로 숫자를 주면 [[NumberData]] 내부 슬롯에 전달받은 숫자를 할당한 Number 객체 생성
- 숫자가 아니라면 숫자로 강제 변환, 불가시엔 NaN 할당

```javascript
const numObj = new Number();
console.log(numObj);
```

## Number 프로퍼티

### Number.EPSILONNumber.EPSILON

Number.EPSILONNumber.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.

```javascript
0.1 + 0.2; // 0.300000000000000000004
0.1 + 0.2 === 0.3; // false
```

위 예제와 같이 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생한다.

```javascript
function isEqual(a,b) {
      // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
	return Math.abs(a-b) < Number.EPSILON; // -< true>
}​
```

### Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. Number.MAX_VALUE보다 큰 숫자는 Infinity다.

### Number.MIN_VALUE

Number.MIN_VALUE는 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다. Number.MIN_VALUE보다 작은 숫자는 0이다.

### Number.MAX_SAFE_INTEGER

Number.MAX_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수 값이다.

### Number.MIN_SAFE_INTEGER

Number.MIN_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수 값이다.

### Number.POSITIVE_INFINITY

Number.POSITIVE_INFINITY는 양의 무한대를 나타내는 숫자 값 Infinity와 같다.

```javascript
Number.MAX_SAFE_INTEGER; // -> 9007199254740991
```

### Number.NEGATIVE_INFINITY

Number.NEGATIVE_INFINITY는 음의 무한대를 나타내는 숫자 값 -Infinity와 같다.

```javascript
Number.MIN_SAFE_INTEGER; // -> -9007199254740991
```

### Number.NaN

Number.NaN는 숫자가 아님을 나타내는 숫자 값이다. Number.NaN은 window.NaN과 같다.

## Number 메서드

### Number.isFinite

Number.isFinite는 인수로 전달된 숫자 값이 정상적인 유한수인지 검사하여 그 결과를 불리언 값으로 반환한다.

```javascript
// 인수가 정상적인 유한수이면 true를 반환한다.
Number.isFinite(0); // -> true
Number.isFinite(Number.MAX_VALUE); // -> true
Number.isFinite(Number.MIN_VALUE); // -> true

// 인수가 무한수이면 false를 반환한다.
Number.isFinite(Infinity); // -> false
Number.isFinite(-Infinity); // -> false
```

인수가 NaN이면 false 반환

```javascript
Number.isFinite(NaN); // -> false
```

### Number.isInteger

Number.isInteger는 인수로 전달된 숫자 값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

### Number.isNaN

Number.isNaN는 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

### Number.isSafeInteger

Number.isSafeInteger는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.여기서 안전한 정수 값은 -(253-1)과 253-1 사이의 정수 값이다.

### Number.prototype.toExponential

Number.prototype.toExponential는 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.

### Number.prototype.toFixed

Number.prototype.toFixed는 숫자를 반올림하여 문자열로 반환한다.

### Number.prototype.toPrecision

Number.prototype.toPrecision는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

```javascript
// 전체 자리수 유효. 인수를 전달하지 않으면 기본값 0이 전달된다.
(12345.6789).toPrecision(); // -> "12345.6789"
// 전체 1자리수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // -> "1e+4"
// 전체 2자리수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // -> "1.2e+4"
// 전체 6자리수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // -> "12345.7"
```

### Number.prototype.toString

Number.prototype.toString는 숫자를 문자열로 변환하여 반환한다.
