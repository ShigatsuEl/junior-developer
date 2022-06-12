# 클래스와 스타일 바인딩

데이터 바인딩의 일반적인 요구사항은 엘리먼트의 클래스 목록과 인라인 스타일을 조작하는 것입니다. 클래스와 스타일 모두 속성이므로, v-bind를 사용하여 처리할 수 있습니다. 표현식으로 최종 문자열을 계산하기만 하면 됩니다. 그러나 문자열 연결을 방해하는 것은 성가시고 오류가 발생하기 쉽습니다. 이러한 이유로 Vue는 v-bind가 class와 style과 함게 사용될 때 특별한 개선사항을 제공합니다. 문자열 외에도 표현식은 객체 또는 배열로 평가될 수 있습니다.

### HTML 클래스 바인딩

#### 객체 구문

`v-bind:class` (v-bind:class의 약자)에 객체를 전달하여 클래스를 동적으로 전환할 수 있습니다.

```html
<div :class="{ active: isActive }"></div>
```

위 구문은 active 클래스의 존재가 데이터 속성 isActive의 진실성(truthiness) (opens new window)에 의해 결정됨을 의미합니다.

객체에 더 많은 필드를 포함하여, 여러 클래스를 전환할 수 있습니다. 또한, :class 지시문은 일반 class속성과 공존할 수도 있습니다. 따라서 다음과 같은 형식을 따르게 됩니다.

```html
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```

다음 데이터에 따라

```javascript
data() {
  return {
    isActive: true,
    hasError: false
  }
}
```

이렇게 렌더링 됩니다.

```html
<div class="static active"></div>
```

또한, 바인딩 된 객체는 인라인일 필요는 없습니다.

```html
<div :class="classObject"></div>
```

```javascript
data() {
  return {
    classObject: {
      active: true,
      'text-danger': false
    }
  }
}
```

이것은 동일한 결과를 렌더링합니다. 객체를 반환하는 computed property에 바인딩 할 수도 있습니다. 이것은 일반적으로 강력한 패턴입니다.

```javascript
data() {
  return {
    isActive: true,
    error: null
  }
},
computed: {
  classObject() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

#### 배열 구문

배열을 `:class`에 전달하여 클래스 목록을 적용할 수 있습니다.

```html
<div :class="[activeClass, errorClass]"></div>
```

```javascript
data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}
```

다음처럼 렌더링됩니다.

```html
<div class="active text-danger"></div>
```

조건부로 목록의 클래스도 전환하려면 삼항 표현식을 사용하여 수행할 수 있습니다.

```html
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```

이는 항상 errorClass를 적용하지만 isActive가 true 인 경우 activeClass만 적용합니다.

그러나, 여러 조건부 클래스가 있다면 이것은 약간 장황할 수 있습니다. 그렇기 때문에 배열 구문 내에서 객체 구문을 사용할 수도 있습니다.

```html
<div :class="[{ active: isActive }, errorClass]"></div>
```

#### 컴포넌트에서 사용하기

단일 루트 엘리먼트가 있는 커스텀 컴포넌트에서 class 속성을 사용하면 해당 클래스가 이 엘리먼트에 추가됩니다. 이 엘리먼트의 기존 클래스는 덮어 쓰지 않습니다.

```javascript
const app = Vue.createApp({});

app.component("my-component", {
  template: `<p class="foo bar">Hi!</p>`,
});
```

또한 사용할 때 몇 가지 클래스를 추가할 수 있습니다.

```html
<div id="app">
  <my-component class="baz boo"></my-component>
</div>
```

렌더링된 HTML은 다음과 같습니다.

```html
<p class="foo bar baz boo">Hi</p>
```

클래스 바인딩을 이용해도 동일합니다.

```html
<my-component :class="{ active: isActive }"></my-component>
```

isActive 가 참일 때, 렌더링된 HTML은 다음과 같습니다.

```html
<p class="foo bar active">Hi</p>
```

컴포넌트에 여러 루트 엘리먼트가 있는 경우 이 클래스를 받을 컴포넌트를 정의해야 합니다. $attrs 컴포넌트 속성을 사용하여 이 작업을 수행할 수 있습니다.

```html
<div id="app">
  <my-component class="baz"></my-component>
</div>
```

```javascript
const app = Vue.createApp({});

app.component("my-component", {
  template: `
    <p :class="$attrs.class">Hi!</p>
    <span>This is a child component</span>
  `,
});
```

### 인라인 스타일 바인딩하기

#### 객체 구문

`:style`의 객체 구문은 매우 간단합니다. JavaScript 객체라는 점을 제외하면 CSS와 거의 비슷합니다. CSS 속성 이름에는 카멜 케이스(camelCase) 또는 케밥 케이스(kebab-case, 케밥 케이스와 함께 따옴표 사용)를 사용할 수 있습니다.

```html
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```javascript
data() {
  return {
    activeColor: 'red',
    fontSize: 30
  }
}
```

템플릿을 더 깔끔하게 만들기 위해, 스타일 객체에 직접 바인딩하는 것이 좋습니다.

```html
<div :style="styleObject"></div>
```

```javascript
data() {
  return {
    styleObject: {
      color: 'red',
      fontSize: '13px'
    }
  }
}
```

#### 배열 구문

`:style`의 배열 구문을 사용하면, 동일한 요소에 여러 스타일 객체를 적용 할 수 있습니다.

```html
<div :style="[baseStyles, overridingStyles]"></div>
```

#### 다중 값

스타일 속성에 다중 값(접두사)의 배열을 제공할 수 있습니다. 예를 들어:

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

이것은 브라우저가 지원하는 배열의 마지막 값만 렌더링합니다. 이 예에서는 접두사가 없는 버전의 flexbox를 지원하는 브라우저에 대해 display: flex를 렌더링합니다.

**이 글을 [Vue.js](https://v3.ko.vuejs.org/) 공식문서를 참고하여 작성되었습니다.**
