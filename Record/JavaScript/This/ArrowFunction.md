# 화살표 함수(Arrow Function)

[이전 글 보러가기 -> This](./This.md)

### 화살표 함수란?

화살표 함수는 `function` 키워드 대신에 화살표 `=>`를 사용하여 보다 간편한 방법으로 함수를 선언할 수 있다.<br>

화살표 함수의 특징은 암시적으로 값을 반환한다는 것과 표현식이기 때문에 호이스팅이 발생하지 않는다는 것이다.<br>

아래와 같은 방법으로 선언할 수 있다.<br>

```
// 매개변수 지정 방법
    () => { ... } // 매개변수가 없을 경우
     x => { ... } // 매개변수가 한 개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... } // 매개변수가 여러 개인 경우, 소괄호를 생략할 수 없다.

// 함수 몸체 지정 방법
x => { return x * x }  // single line block
x => x * x             // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다.

() => { return { a: 1 }; }
() => ({ a: 1 })  // 위 표현과 동일하다. 객체 반환시 소괄호를 사용한다.

() => {           // multi line block.
  const x = 10;
  return x * x;
};
```

화살표 함수는 콜백함수로 사용하면 굉장히 편리하지만 일반 함수와 큰 차이점이 하나 존재해 모르고 사용하면 자신도 모르는 에러를 발생시킬 수 있다.<br>

그것은 바로 this이다.<br>

### 어휘적 this

일반적인 함수는 저번 글에서 언급한 4가지 규칙을 준수한다.<br>

하지만 이 규칙들을 따르지 않는 특별한 함수가 존재하는데 바로 **화살표 함수** 이다.<br>

일반 함수에서 this는 함수가 호출할 때 어떻게 호출되는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.<br>

하지만 화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정된다.<br>

즉, 화살표 함수가 선언된 환경에 따라 this의 값이 결정된다는 것을 의미한다.<br>

게다가 이 화살표 함수는 call, apply, bind 메서드로 바인딩을 오버라이딩 할 수 없으며 new 생성자로도 오버라이딩 할 수 없습니다.<br>

아래 코드를 통해 확인해 봅시다.<br>

```
function foo() {
  // 화살표 함수를 반환
  return (a) => {
    console.log(this.a); // 여기서 this는 어휘적으로 foo()에서 상속된다.
  }
}

var obj1 = {
  a: 2
}
var obj2 = {
  a: 3
}
var bar = foo.call(obj1);
bar.call(); // 2
```

화살표 함수는 어휘적으로 this가 결정된다. 좀 더 자세히 설명하면 화살표 함수는 일반 함수와 달리 자신의 상위 스코프의 this를 따른다는 것이다.<br>

위 코드에서 foo() 함수의 화살표 함수의 상위 스코프는 foo() 함수이다. 때문에 화살표 함수의 this는 그 상위 스코프인 foo()함수에서 this와 동일한 obj1과 같다.<br>

따라서 결과값은 2가 나오게 된다.<br><br>

`즉, 일반 함수와 달리 화살표 함수의 this는 언제나 상위 스코프의 this를 가리키며 이것을` **Lexical this** `라고 한다.`<br>

화살표 함수는 이처럼 축약해서 사용하기 때문에 간편하다는 장점도 있지만 일반 함수와 화살표 함수를 혼용해서 사용하면 바인딩 된 this가 무엇인지 알기 헷갈릴 것이다.<br>

또한 아래와 같은 경우 화살표 함수를 사용하게 되면 오히려 문제가 되기도 한다.<br>

- 객체의 메서드로 사용

- 프로토타입의 메서드로 사용

- 생성자 함수로 사용

- 이벤트 리스너 콜백 함수로 사용

위와 같은 상황에서 this를 사용하면 상위 스코프인 글로벌 객체를 this가 바인딩하게 되므로 값이 존재하지 않을 수 있다.<br>

이러한 경우는 일반 함수로 지정해 this를 사용하는 것이 좋으며 일반 함수와 화살표 함수를 혼용해서 사용할 경우엔 일반 함수를 사용할 경우와 화살표 함수를 사용할 경우를 구분지어서 사용해야 this의 바인딩을 헷갈리지 않고 유추할 수 있습니다.<br>

[다음 글로 이동하기 -> Execution Context(실행 컨텍스트)](../Context/Context.md)
