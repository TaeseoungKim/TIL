![](https://i.imgur.com/LOqoalW.png)

본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# Set과 Map

## Set

중복되지 않는 유일한 값들의 집합
객체임

### Set 객체의 생성

Set 생성자 함수로 생성
인수로 전달받은 이터러블에서 중복된 값은 제외하고 Set 객체 생성한다

```javascript
const set = new Set();
const set1 = new Set([1, 2, 1]); // Set(2) {1,2}
const set2 = new Set("banana"); // Set(3) {'b','a','n'}
```

Set로 배열 중복 요소 제거 가능

```javascript
const unique = (arr) => [...new Set(arr)];
console.log(unique([1, 2, 1])); // [1,2]
```

### 요소 개수 확인

Set.prototype.size로 확인
size프로퍼티(접근자 프로퍼티)는 setter 함수가 없기 때문에 할당해도 변하지 않음

```javascript
const set = new Set([1, 2, 1]);
console.log(set.size); // 2
set.size = 100;
console.log(set.size); // 2
```

### 요소 추가

Set.prototype.add 메서드로 추가
add메서드는 Set객체에 요소를 추가한 새로운 객체를 반환 -> add().add() 가능
이미 Set객체에 있는 요소를 추가하면 추가하지 않고 에러는 발생하지 않는다
0과 -0, NaN과 NaN을 같다고 보고 중복 추가하지 않는다
자바스크립트의 모든 값을 Set 객체의 요소로 저장할 수 있다

```javascript
const set = new Set();
set.add(1).add(2).add(3);
console.log(set); // Set(3) {1,2,3}
set.add(1).add(1).add(1);
console.log(set); // Set(3) {1,2,3}
```

### 요소 존재 여부 확인

Set.prototype.has 메서드로 확인

```javascript
const set = new Set([1, 2]);
set.has(1); // true
set.has(4); // false
```

### 요소 삭제

Set.prototype.delete 메서드로 삭제
인수로 삭제할 요소를 전달
delete 메서드의 반환값은 삭제 성공여부
삭제시 true, 아닐 시 false
즉 없는 요소를 삭제하면 에러가 아닌 false 반환
Set.prototype.add 의 경우 추가된 객체를 반환하지만 delete는 반환값이 성공여부이기 때문에 .delete().delete() 불가능

```javascript
const set = new Set([1, 2, 3]);
set.delete(1); // true
set.delete(5); // false
console.log(set); // Set(2) {2,3}
```

### 요소 일괄 삭제

Set.prototype.clear 로 초기화
반환값은 undefined

```javascript
const set = new Set([1, 2, 3, 4, 5]);
set.clear(); // undefined
console.log(set); // Set(0) {}
```

### 요소 순회

Set.prototype.forEach 메서드로 요소 순회
Array.prototype.forEach와 비슷하다
Array.prototype.forEach에 인수로 전달하는 콜백함수 안에 세개의 인수는 요소,인덱스,객체자체 인데
Set는 인덱스가 없으므로 요소,요소,객체자체

```javascript
const set = new Set([1,2,3]);
set.forEach((item,noIdx,self) => console.log(item,noIdx,self));
/_
1 1 set(3) {1,2,3}
2 2 set(3) {1,2,3}
3 3 set(3) {1,2,3}
_/
```

Set 객체는 이터러블임 -> for...of, 스프레드문법, 디스트럭처링 할당 가능
순회 하는 순서는 추가된 순서와 같다
Set객체 내에서 순서가 의미는 없지만 다른 이터러블과 호환성을 유지하기 위해

### 집합 연산

집합 연산을 프로토타입 메서드로 구현해보면

교집합

```javascript
Set.prototype.intersection = function (otherSet) {
  return new Set([...this].filter((x) => otherSet.has(x)));
};
```

합집합

```javascript
Set.prototype.union = function (otherSet) {
  return new Set([...this, ...otherSet]);
};
```

차집합

```javascript
Set.prototype.difference = function (otherSet) {
  return new Set([...this].filter((x) => !otherSet.has(x)));
};
```

부분집합

```javascript
Set.prototype.isSuperset = function (otherSet) {
  for (const item of this) {
    if (!otherSet.has(item)) return false;
  }
  return true;
};
```

## Map

키와 값의 쌍으로 이루어진 컬렉션
객체와 유사하지만 객체는 키로 문자열,심벌 값만 가능한데
Map 객체는 키로 모든 값 가능
Map 객체는 이터러블임

### Map 객체의 생성

Map 생성자 함수로 생성
키와 값의 쌍으로 이루어진 요소로 구성된 이터러블을 인수로 전달받는다
중복된 키를 갖는 요소가 있으면 덮어 쓴다

```javascript
const map = new Map();
const map2 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);

const map3 = new Map([
  ["key1", "value1"],
  ["key1", "value2"],
]);
console.log(map3); // Map(1) {'key1' => 'value2'}
```

### 요소 개수 확인

Map.prototype.size로 확인
getter만 있는 접근자 프로퍼티임
값 할당해도 변화하지 않음

```javascript
const map = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(map.size); // 2
map.size = 100;
console.log(map.size); // 2
```

Map.prototype.set 메서드로 추가
set 메서드는 새로운 Map객체를 반환하기 때문에 .set().set() 가능
키값으로 0,-0 NaN,NaN 은 같기 때문에 덮어씌움

```javascript
const map = new Map();
map.set('key1','value1');
키 값으로 모든 값 사용 가능

const map = new Map();
const p1 = {name:'aj'};
const p2 = {name:'mj'};
map.set(p1,'male');
map.set(p2,'female');
```

### 요소 취득

Map.prototype.get 메소드로 취득
인수로 키 값을 전달, 해당 키를 갖는 요소가 없을 시 undefined를 반환

```javascript
const map = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
map.get("key1"); // 'value1'
```

### 요소 존재 여부 확인

Map.prototype.has 메서드로 확인
인수로 키 값을 전달
있으면 true, 없으면 false 반환

### 요소 삭제

Map.prototype.delete 메서드로 삭제
인수로 전달받은 값을 키로 갖는 요소를 삭제
반환값은 성공시 true, 실패시 false(에러는 안남)

### 요소 일괄 삭제

Map.prototype.clear 로 밀어버린다
반환값은 undefined

### 요소 순회

Map.prototype.forEach 메서드로 순회
콜백 함수 내부의 3가지 인수는 요소값,요소의 키, Map객체 자체
Map 객체는 이터러블이기 때문에 for...of, 스프레드 문법, 디스트럭처링 할당 가능

이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공

- Map.prototype.keys
- Map.prototype.values
- Map.prototype.entries
