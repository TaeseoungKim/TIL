![](https://i.imgur.com/LOqoalW.png)

본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 배열

자바스크립트에 배열이라는 타입은 존재하지 않는다. **배열은 객체 타입**이다.
배열의 프로토타입 객체는 Array.prototype이다. Array.prototype은 배열을 위한 빌트인 메서드를 제공한다.

```javascript
const arr = [1, 2, 3];

arr.constructor === Array; // -> true
Object.getPrototypeOf(arr) === Array.prototype; // -> true
```

배열은 객체지만 일반 객체와는 구별되는 특징이 있다.

| 구분            | 객체                      | 배열          |
| --------------- | ------------------------- | ------------- |
| 구조            | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조       | 프로퍼티 키               | 인덱스        |
| 값의 순서       | X                         | O             |
| length 프로퍼티 | X                         | O             |

## 자바스크립트 배열은 배열이 아니다

**밀집 배열(일반적인 배열)**
동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조로, 하나의 데이터 타입으로 통일되어 있음

**희소 배열(자바스크립트 배열)**
배열의 요소를 위한 각 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다. 즉, 배열의 동작을 흉내 낸 특수한 객체

```javascript
// "16.2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체" 참고
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': {value: 1, writable: true, enumerable: true, configurable: true}
  '1': {value: 2, writable: true, enumerable: true, configurable: true}
  '2': {value: 3, writable: true, enumerable: true, configurable: true}
  length: {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```

위 예제와 같이 자바스크립트 배열은 인덱스를 나타내는 **문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는다.**
즉, 배열의 요소가 프로퍼티 값이므로 어떤 타입의 값이든 배열의 요소가 될 수 있다.

일반적인 배열: 인덱스를 통해 요소에 빠르게 접근 가능, but 요소 삽입 또는 삭제시 비효율적
자바스크립트 배열: 해시 테이블로 구현된 객체로 요소에 접근이 느림, but 요소를 삽입 또는 삭제하는 경우에 빠른 성능

대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구별하여 좀 더 배열처럼 동작하도록 최적화하여 구현했다.
아래 예제를 보면 배열이 일반 객체보다 약 2배 빠르다.

```javascript
const arr = [];

console.time("Array Performance Test");

for (let i = 0; i < 10000000; i++) {
  arr[i] = i;
}
console.timeEnd("Array Performance Test");
// 약 340ms

const obj = {};

console.time("Object Performance Test");

for (let i = 0; i < 10000000; i++) {
  obj[i] = i;
}

console.timeEnd("Object Performance Test");
// 약 600ms
```

## length 프로퍼티와 희소 배열

희소배열: 연속적으로 밀집되어 있지 않은 배열

**희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.**
희소 배열의 length 값은 실제 요소 개수보다 언제나 크다.

```javascript
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 4, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

### Array.from

ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.
Array.from을 사용하면 두 번쨰 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있다.

Array.from메서드는 두 번쨰 인수로 전달한 콜백 함수에 첫 번쨰 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환한다.

```javascript
// Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소로 채운다.
Array.from({ length: 3 }); // -> [undefined, undefined, undefined]

// Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
Array.from({ length: 3 }, (_, i) => i); // -> [0, 1, 2]
```

tip) **유사배열 객체**
마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. 유사 배열 객체는 마치 배열처럼 for 문으로 순회할 수도 있다.

tip) **이터러블 객체**
Array, String, Map, Set, arguments 등..

## 배열 요소의 추가와 갱신

객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가할 수 있다.
존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.

인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수를 사용해야 한다. 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다. 이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.

```javascript
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr["1"] = 2;

// 프로퍼티 추가
arr["foo"] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

## 배열 요소의 삭제

배열은 객체이기 때문에 delete 연산자를 사용할 수 있다.
delete 연산자는 객체의 프로퍼티를 삭제한다. **이때 배열은 희소 배열이 되며 length 프로퍼티 값은 변하지 않는다. 따라서 희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋다.**

```javascript
const arr = [1, 2, 3];

// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않는다. 즉, 희소 배열이 된다.
console.log(arr.length); // 3
```

**희소 배열을 만들지 않으면서 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.**

```javascript
const arr = [1, 2, 3];

// Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
// arr[1]부터 1개의 요소를 제거
arr.splice(1, 1);
console.log(arr); // [1, 3]

// length 프로퍼티가 자동 갱신된다.
console.log(arr.length); // 2
```

Array 생성자 함수는 정적 메서드를 제공하며, 배열 객체의 프로토타입인 Array.prototype은 프로토타입 메서드를 제공한다. 배열은 사용 빈도가 높은 자료구조이므로 배열 메서드의 사용법을 잘 알아둘 필요가 있다.

배열 메서드는 결과물을 반환하는 패턴이 두 가지이므로 주의가 필요하다.

1. 원본 배열을 직접 변경하는 매서드
2. 원본 배열을 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드

원본 배열을 직접 변경하는 매서드는 외부 상태를 직접 변경하는 부수 효과가 있으므로 가급적 **원본 배열을 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드를 사용하는 것이 좋다.**

```javascript
const arr = [1];

// push 메서드는 원본 배열(arr)을 직접 변경한다.
arr.push(2);
console.log(arr); // [1, 2]

// concat 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat(3);
console.log(arr); // [1, 2]
console.log(result); // [1, 2, 3]
```

## 배열 메서드

### indexOf와 includes

**Array.prototype.indexOf**

```javascript
const arr = [1, 2, 2, 3];

// 배열 arr에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환한다.
arr.indexOf(2); // -> 1
// 배열 arr에 요소 4가 없으므로 -1을 반환한다.
arr.indexOf(4); // -> -1
// 두 번째 인수는 검색을 시작할 인덱스다. 두 번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2); // -> 2
```

**Array.prototype.includes**
indexOf 보다 includes 메서드가 가독성이 더 좋다.

```javascript
const foods = ["apple", "banana", "orange"];

// foods 배열에 'orange' 요소가 존재하는지 확인한다.
if (foods.indexOf("orange") === -1) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가한다.
  foods.push("orange");
}

console.log(foods); // ["apple", "banana", "orange"]
```

### Array.prototype.push

**push**
push 메서드는 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가한다.

```javascript
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열 arr의 마지막 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.push(3, 4);
console.log(result); // 4

// push 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 2, 3, 4]
```

push 메서드는 성능면에서 좋지 않으므로 아래와 같이 **length 프로퍼티를 이용하거나 스프레드 문법을 사용하는 것이 좋다.**

```javascript
const arr = [1, 2];

// arr.push(3)과 동일한 처리를 한다. 이 방법이 push 메서드보다 빠르다.
arr[arr.length] = 3;
console.log(arr); // [1, 2, 3]
```

```javascript
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [...arr, 3];
console.log(newArr); // [1, 2, 3]
```

### Array.prototype.unshift

unshift 메서드는 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가한다.

```javascript
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.unshift(3, 4);
console.log(result); // 4

// unshift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 4, 1, 2]
```

마찬가지로 unshift 함수보다 ES6 스프레드 문법을 사용하는 것이 좋다.(부수효과 방지)

```javascript
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [3, ...arr];
console.log(newArr); // [3, 1, 2]
```

### Array.prototype.shift

원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.

```javascript
const arr = [1, 2];

// 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.shift();
console.log(result); // 1

// shift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [2]
```

### Array.prototype.concat

인수로 전달된 값을(배열 또는 원시값)을 원본 배열의 마지막 요소에 추가한 새로순 배열을 반환한다.
**원본 배열은 변경되지 않는다.**

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
// 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(3);
console.log(result); // [1, 2, 3]

// 배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]
```

### Array.prototype.splice

원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거한다.
splice 메서드는 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.

- start: 원본 배열의 요소를 제거하기 시작할 인덱스다. start만 지정하면 원본 배열의 start부터 모든 요소를 제거한다. start가 음수인 경우 배열의 끝에서의 인덱스를 나타낸다. 만약 start가 -1이면 마지막 요소를 가리키고 -n이면 마지막 요소를 가리키고 -n이면 마지막에서 n번쨰 요소를 가리킨다.

- deleteCount: 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수다. 0인 경우 아무런 요소도 제거되지 않는다.

- items: 제거한 위치에 삽입할 요소들의 목록이다. 생략할 경우 원본 배열에서 요소들을 제거하기만 한다.

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

splice 메서드의 두 번째 인수, 즉 제거할 요소의 개수를 생략하면 첫 번째 인수로 전달된 시작 인덱스부터 모든 요소를 제거한다.

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 모든 요소를 제거한다.
const result = arr.splice(1);

// 원본 배열이 변경된다.
console.log(arr); // [1]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3, 4]
```

### Array.prototype.slice

slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. **splice와 헷갈림 주의**
두 개의 매개변수를 갖는다. (start, end)

end 생략시 start부터 모든 요소 복사하여 배열로 반환한다.

### Array.from 메서드

Array.from 메서드를 사용하면 더욱 간단하게 유사 배열 객체를 배열로 변환할 수 있다. Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 배열로 변환한다.

```javascript
function sum() {
  const arr = Array.from(arguments);
  console.log(arr); // [1, 2, 3]

  return arr.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

### Array.prototype.join

join 메서드는 원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열 반환
기본 구분자는 콤마다.(',')

```javascript
const arr = [1, 2, 3, 4];

// 기본 구분자는 ','이다.
// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 기본 구분자 ','로 연결한 문자열을 반환한다.
arr.join(); // -> '1,2,3,4';

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈문자열로 연결한 문자열을 반환한다.
arr.join(""); // -> '1234'

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환한다.ㄴ
arr.join(":"); // -> '1:2:3:4'
```
