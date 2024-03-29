# 컴포넌트 등록

### 컴포넌트 이름

컴포넌트를 등록할 때 항상 이름을 등록해야 합니다. 예를 들어, 이때까지 봤던 전역 등록의 경우

```javascript
const app = Vue.createApp({...})

app.component('my-component-name', {
  /* ... */
})
```

컴포넌트의 이름은 app.component의 첫 번째 인자입니다. 위 예시의 경우 컴포넌트 이름은 "my-component-name" 입니다.

컴포넌트의 이름은 사용하고자 하는 의도에 따라서 정해져야 할 수 있습니다. 만약 컴포넌트를 DOM에 직접 사용하고자 하는 경우(string template이나 single-file component을 사용하지 않는 경우), 커스텀 태그의 이름을 지을 때 다음 W3C 규칙 (opens new window)을 따르기를 강력히 권장합니다.

- 영문 소문자만 사용
- 하이픈 이용(여러 단어를 하이픈으로 연결해 사용)

#### 이름 대소문자 규칙

컴포넌트를 스트링 템플릿이나 단일 파일 컴포넌트(SFC; single-file component)로 정의하는 경우, 컴포넌트 이름을 두 가지 방식으로 정의할 수 있습니다.

- kebob-case 로 정의하기

  ```javascript
  app.component("my-component-name", {
    /* ... */
  });
  ```

  컴포넌트를 kebab-case로 정의하는 경우, 커스텀 엘리먼트로써 컴포넌트를 참조하는 경우에도 kebab-case를 사용하여야 합니다. 즉, <my-component-name>의 형태로 사용하여야 합니다.

- 파스칼케이스로 정의하기

  ```javascript
  app.component("MyComponentName", {
    /* ... */
  });
  ```

  컴포넌트를 PascalCase로 정의하는 경우에는 커스텀 엘리먼트로써 컴포넌트를 참조할 때 두 가지 방식을 모두 이용할 수 있습니다. 즉, <my-component-name> 와 <MyComponentName> 두 가지 모두 사용이 가능합니다. 이 때, DOM에 직접 사용되는 경우(i.e. non-string 템플릿)에는 kebab-case로 정의된 이름만 사용 가능하다는 점을 알아두세요.

### 전역 등록

이제까지 우리는 app.component를 이용해서 컴포넌트를 생성하였습니다.

```javascript
Vue.createApp({...}).component('my-component-name', {
  // ... options ...
})
```

이 컴포넌트들은 애플리케이션에 전역 등록 됩니다. 이는 모든 컴포넌트 인스턴스의 템플릿 내부에 사용될 수 있다는 뜻입니다.

```javascript
const app = Vue.createApp({});

app.component("component-a", {
  /* ... */
});
app.component("component-b", {
  /* ... */
});
app.component("component-c", {
  /* ... */
});

app.mount("#app");
```

```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```

### 지역 등록

전역 등록은 보통 이상적이지 않습니다. 예를 들어 Webpack과 같은 빌드 시스템을 사용하는 경우 **컴포넌트를 전역 등록하는 것은 컴포넌트를 사용하지 않더라도 계속해서 최종 빌드에 해당 컴포넌트가 포함되는 것을 의미**합니다. 이는 사용자가 다운로드하는 자바스크립트 파일의 크기를 불필요하게 증가시킵니다.

이러한 경우, 컴포넌트를 순수한 자바스크립트 객체로써 정의할 수 있습니다.

```javascript
const ComponentA = {
  /* ... */
};
const ComponentB = {
  /* ... */
};
const ComponentC = {
  /* ... */
};
```

그 다음, 컴포넌트를 components 옵션 안에 정의하세요.

```javascript
const app = Vue.createApp({
  components: {
    "component-a": ComponentA,
    "component-b": ComponentB,
  },
});
```

components 객체 내에 있는 각 속성의 키는 커스텀 객체의 이름이 되고, 값은 컴포넌트 오브젝트를 포함하게 됩니다.

이 때, **지역적으로 등록된 컴포넌트는 서브 컴포넌트에서의 사용이 불가하다는 점을 기억하세요.** 예를 들어, ComponentA가 ComponentB 내부에서 사용될 수 있게 하려면 아래와 같이 사용하여야 합니다.

```javascript
const ComponentA = {
  /* ... */
};

const ComponentB = {
  components: {
    "component-a": ComponentA,
  },
  // ...
};
```

만약 Babel이나 Webpack같은 ES2015 모듈을 사용하는 경우, 아래와 같이 사용하는게 더 자연스러울 수도 있습니다.

```javascript
import ComponentA from "./ComponentA.vue";

export default {
  components: {
    ComponentA,
  },
  // ...
};
```

ES2015+ 에서 ComponentA와 같은 변수 이름을 객체에 그대로 사용하는 것은 ComponentA: ComponentA와 같이 사용하는 것이 됩니다. 즉, 아래 두 개의 의미를 갖게 됩니다.

- 템플릿에 사용한 커스텀 엘리먼트의 이름
- 컴포넌트 옵션이 될 변수의 이름

### 모듈 시스템

#### 모듈 시스템에서의 지역 등록

만약 계속 이 문단을 읽고 있다면 Babel이나 Webpack같은 모듈 시스템을 사용하고 있다는 뜻일 것입니다. 이러한 경우, 여러 컴포넌트를 한번에 담고 있는 components 디렉토리를 만드는 것을 권장합니다.

그 다음, 지역적으로 컴포넌트를 등록하는 대신 사용할 각 컴포넌트를 모두 import합니다. 예를 들어, ComponentB.js 혹은 ComponentB.vue라는 파일이 있다고 할 때, 아래와 같이 작성합니다.

```javascript
import ComponentA from "./ComponentA";
import ComponentC from "./ComponentC";

export default {
  components: {
    ComponentA,
    ComponentC,
  },
  // ...
};
```

**이 글을 [Vue.js](https://v3.ko.vuejs.org/) 공식문서를 참고하여 작성되었습니다.**
