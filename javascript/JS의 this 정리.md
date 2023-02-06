## JavaScript 함수의 this 바인딩

Javascript의 this는 상황에 따라 다르게 바인딩된다.
대표적으로 this에 바인딩되는 값들은 아래와 같다.

전역 공간의 this: 전역 객체
메소드 호출 시 메소드 내부의 this: 해당 메소드를 호출한 객체
함수 호출 시 함수 내부의 this: **지정되지 않는다.(Wtf....)**

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

당연히 this.name은 cat 객체의 내부에 지정한 'meow'일 줄 알았는데 undefined라니! 함수를 호출할 때 그 함수를 호출한 배경의 상황이 this에 담기면 정말 좋을텐데 말이죠. 그런데.... 놀랍게도 그것을 화살표 함수가 해냅니다.

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```
