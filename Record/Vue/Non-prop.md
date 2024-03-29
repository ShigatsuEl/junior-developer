# Non-Prop 속성

컴포넌트 non-prop 속성은 컴포넌트에 전달되지만, props나 emits에 정의된 특성을 지니고 있지 않은 속성 또는 이벤트 리스너를 의미합니다. 이에 대한 일반적인 예로 class, style, id 속성이 있습니다. 이러한 속성들은 $attrs 프로퍼티를 통해 접근할 수 있습니다.

### 속성 상속

```javascript
app.component("date-picker", {
  template: `
    <div class="date-picker">
      <input type="datetime">
    </div>
  `,
});
```

data-status 속성을 통해 date-picker 컴포넌트 상태를 정의할 때, 해당 속성은 컴포넌트의 루트 노드에 적용됩니다. (예: div.date-picker)

```html
<!-- non-prop 속성과 Date-picker 컴포넌트 -->
<date-picker data-status="activated"></date-picker>

<!-- 렌더링된 date-picker 컴포넌트 -->
<div class="date-picker" data-status="activated">
  <input type="datetime" />
</div>
```

이벤트 리스너에도 동일한 규칙이 적용됩니다.

```html
<date-picker @change="submitChange"></date-picker>
```

```javascript
app.component("date-picker", {
  created() {
    console.log(this.$attrs); // { onChange: () => {}  }
  },
});
```

### 속성 상속 비활성화

만약 컴포넌트가 속성을 자동으로 상속하지 않도록 하려면, 컴포넌트 옵션에서 `inheritAttrs: false`를 설정하세요.

속성 상속을 비활성화하는 일반적인 시나리오는 속성을 루트 노드 이외의 다른 엘리먼트들에 적용해야 하는 경우입니다.

inheritAttrs 옵션을 false로 설정하면, props와 emits 속성에 포함되지 않은 모든 속성(예: class, style, v-on 리스너 등)을 포함하고 있는 컴포넌트의 `$attrs` 속성을 사용하여 다른 엘리먼트에 속성을 적용하도록 제어할 수 있습니다.

이전 섹션의 date-picker 컴포넌트 예제에서 루트 div 엘리먼트 대신 input 엘리먼트에 모든 non-prop 속성을 적용해야 하는 경우, v-bind를 사용하여 작업을 수행할 수 있습니다.

```javascript
app.component("date-picker", {
  inheritAttrs: false,
  template: `
    <div class="date-picker">
      <input type="datetime" v-bind="$attrs">
    </div>
  `,
});
```

새 구성으로 data-status 속성이 input 엘리먼트에 적용될 것입니다!

```html
<!-- non-prop 속성과 Date-picker 컴포넌트 -->
<date-picker data-status="activated"></date-picker>

<!-- 렌더링된 date-picker 컴포넌트 -->
<div class="date-picker">
  <input type="datetime" data-status="activated" />
</div>
```

### 다중 루트 노드의 속성 상속

단일 루트 노드 컴포넌트와 달리 다중 루트 노드 컴포넌트는 자동으로 속성을 아래로 전달하는 동작(fallthrough behavior)을 하지 않습니다. $attrs가 명시적으로 바인딩되지 않으면, 런타임 경고가 발생합니다.

```html
<custom-layout id="custom-layout" @click="changeValue"></custom-layout>
```

```javascript
// 경고가 발생할 것입니다.
app.component('custom-layout', {
  template: `
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  `
})

// 경고 없이 $attrs가 <main> 엘리먼트에 전달됩니다.
app.component('custom-layout', {
  template: `
    <header>...</header>
    <main v-bind="$attrs">...</main>
    <footer>...</footer>
  `
})</main>
```

**이 글을 [Vue.js](https://v3.ko.vuejs.org/) 공식문서를 참고하여 작성되었습니다.**
