## JavaScript 함수의 this 바인딩

Javascript의 this는 상황에 따라 다르게 바인딩된다.
대표적으로 this에 바인딩되는 값들은 아래와 같다.

- 전역 공간의 this: 전역 객체
- 메소드 호출 시 메소드 내부의 this: 해당 메소드를 호출한 객체
- 함수 호출 시 함수 내부의 this: **지정되지 않는다.(Wtf....)**

1번과 2번은 그럴듯해 보이지만 3번이 좀 이상하다.
함수를 호출했을 때 그 함수 내부의 this는 지정되지 않는다. 그리고 this가 지정되지 않은 경우, this는 자동으로 전역 객체를 바라보기 때문에 함수를 호출하면 함수 내부에서의 this는 전역 객체가 된다고 정리할 수 있다.

아쉽게도 자바스크립트 개발자 중 한 명인 더글라스 크락포드조차 이 점은 설계상의 오류라고 지적했다.🥲

그러니까, 그냥 함수를 호출한다면 다음과 같이 이상한 상황이 연출된다.

```javascript
const cat = {
  name: "meow",
  foo1: function () {
    const foo2 = function () {
      console.log(this.name);
    };
    foo2();
  },
};

cat.foo1(); // undefined
```

- cat.foo1() 메소드 호출 시 내부 함수 foo2가 실행됨
- 함수가 호출됐으므로 foo2 내부의 this는 지정되지 않아서 곧 전역 객체를 가리킴
- 전역 객체에 name이란 속성은 존재하지 않으므로 undefined가 뜸

당연히 this.name은 cat 객체의 내부에 지정한 'meow'일 줄 알았는데 undefined라니! 함수를 호출할 때 그 함수를 호출한 배경의 상황이 this에 담기면 정말 좋을텐데 말이다....

이때, 이를 해결할 수 있는 것이 **화살표 함수**이다!

```javascript
const cat = {
  name: "meow",
  foo1: function () {
    const foo2 = () => {
      console.log(this.name);
    };
    foo2();
  },
};

cat.foo1(); // meow
```

위 코드와 달라진 점은 cat 객체의 내부함수 foo2가 화살표 함수로 선언됐다는 점 뿐입니다. 그런데 이번엔 우리가 의도한대로 meow가 잘 찍혔습니다. 어떻게 가능한걸까요?

## 화살표 함수의 this 바인딩

이게 가능한 이유는 화살표 함수에는 **⭐️this가 아예 없기 때문⭐️**이다. 즉, function으로 선언한 함수를 실행할 땐 this가 존재하긴 하지만 값을 지정하지 않는데, 화살표 함수로 선언한 함수에는 this가 없다. ~~아~ 있었는데? 아니 그냥 없어요.~~

JavaScript에서는 어떤 식별자(변수)를 찾을 때 현재 환경에서 그 변수가 없으면 바로 상위 환경을 검색한다. 그렇게 점점 상위 환경으로 타고 타고 올라가다가 변수를 찾거나 가장 상위 환경에 도달하면 그만두게 되는 것이다. **화살표 함수에서의 this 바인딩 방식도 이와 유사하다. 화살표 함수에는 this라는 변수 자체가 존재하지 않기 때문에 그 상위 환경에서의 this를 참조하게 된다.**

더 정확히는, function으로 선언한 함수가 메소드로 호출되냐 함수 자체로 호출되냐에 따라 동적으로 this가 바인딩되는 반면, 화살표 함수는 선언될 시점에서의 상위 스코프가 this로 바인딩됩니다.(메소드 호출 시 해당 메소드를 호출한 객체, 함수 자체로 호출 시 함수 내부의 this는 전역)

조금 어렵고 헷갈리실 수 있다. 그냥 편하게 화살표 함수를 쓰면 내가 의도한 바로 그 this가 바인딩되는구나! 하고 생각해도 개발하는 데에는 별 지장 없을 것 같다!😇

## 이럴 땐 화살표 함수를 쓰면 안돼요

이렇게 JavaScript의 함수 this 바인딩 문제를 깔끔하게 해결해 준 화살표 함수도 사용해선 안되는 때가 있다. 상위 환경의 this를 참조한다는 점이 문제가 될 수도 있기 때문이다. 바로 다음과 같은 경우!

### 1. 메소드

```javascript
const cat = {
name: 'meow';
callName: () => console.log(this.name);
}

cat.callName(); // undefined
```

이 같은 경우, callName 메소드의 this는 자신을 호출한 객체 cat이 아니라 함수 선언 시점의 상위 스코프인 전역객체를 가리키게 된다. 어차피 일반 함수를 사용해도 메소드로 호출하면 자신을 호출한 객체를 가리키기 때문에 메소드에서 화살표 함수를 쓸 필요는 없겠죠?😉

### 2. 생성자 함수

```javascript
const Foo = () => {};
const foo = new Foo() // TypeError: Foo is not a constructor
화살표 함수를 생성자함수로 사용하면 에러가 납니다. 생성자 함수로는 사용할 수 없게 만들어졌어요!
```

### 3. addEventListener()의 콜백함수

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

원래 addEventListener의 콜백함수에서는 this에 해당 이벤트 리스너가 호출된 엘리먼트가 바인딩되도록 정의되어 있습니다. 이처럼 이미 this의 값이 정해져있는 콜백함수의 경우, 화살표 함수를 사용하면 기존 바인딩 값이 사라지고 상위 스코프(이 경우엔 전역 객체)가 바인딩되기 때문에 의도했던대로 동작하지 않을 수 있습니다. 물론 상위 스코프의 속성들을 쓰려고 의도한 경우라면 사용할 수 있습니다.
