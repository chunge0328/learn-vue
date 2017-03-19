事件处理器
---

### 一、v-on

使用 `v-on` 监听 `DOM` 事件，只要是原生javascript所支持的事件都可以监听：

```html
<p v-on:click="getSomething">获取数据</p> //true
<p v-on:tap="getSomething">获取数据</p> //false 原生js没有tap事件
```

```js
methods: {
    getSomething: function () {
        console.log('hello Vue');
    }
}
```

可以使用 `@` 替换 `v-on`（推荐）

```html
<p @click="getSomething"></p>
```

也可以使用内联javascript语句，注意：可以是一段javascript代码：

```html
<button @click="var foo = 1; var bar = 2; alert( a + b )"></button>   //alert 3
```

但是一般不建议这么做，而是将事件处理写入一个函数中。

另外，事件函数名不要用javascript保留字：

```html
<p @click="do">hello</p>
```

```js
methods: {
    do: function (event) {
        console.log(event)
    }
}
```

这在生产版本中vue不能正确渲染，但也不会有任何报错，但如果你用的是开发版本的vue.js，那就不用担心了，Vue会温馨的提示你：

`avoid using JavaScript keyword as property name: "do" in expression @click="do"`

<br>

### 二、事件对象

在事件处理函数中，可以用 `event` 形参表示一个事件对象：

```html
<input @input="change">
<p>{{inputText}}</p>
```

```js
data: {
    inputText: ''
},
methods: {
    change: function (event) {
        var value = event.target.value;
        this.inputText = value;
    }
}
```

当在输入框输入的时候，下方的 `p` 标签会同步显示：[演示](http://lavyun.github.io/learn-vue/vue/8-事件处理器/demo1.html)

<br>

### 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在 `methods` 中轻松实现这点，但更好的方式是：`methods` 只有纯粹的数据逻辑，而不是去处理 `DOM` 事件细节。
为了解决这个问题， Vue.js 为 `v-on` 提供了 事件修饰符。通过由点(.)表示的指令后缀来调用修饰符。

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`

```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
```

<br>

### 按键修饰符

在监听键盘事件时，我们经常需要监测常见的键值。 Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

```html
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

- `.enter`
- `.tab`
- `.delete` (捕获 “删除” 和 “退格” 键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

可以通过全局 `config.keyCodes` 对象自定义按键修饰符别名：

```html
// 可以使用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
