条件渲染
----

### 一、v-if

在Vue中，用 `v-if` 实现if渲染

```html
<div v-if="ok">Yes</div>
```
```javascript
data:{
    ok: true
}
```

<br>

也可以用 `v-else` 与 `v-if` 结合使用

```html
<div v-if="ok">Yes</div>
<div v-else>No</div>
```

<br>

甚至可以用 `v-else-if` ，用在 `v-if` 和 `v-else` 之间:

```html
<p v-if="type === 'primary'">primary</p>
<p v-else-if="type === 'success'">success</p>
<p v-else>default</p>
```

<br>

如果我们想切换多个元素呢？可以用 `<temoplate>` 包装：

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```
最终的渲染结果不会包含 `template`

<br>

### 二、使用key控制元素的可重用

当使用`v-if`进行条件切换时，元素是不会摧毁的，比如官方的这个[示例](https://cn.vuejs.org/v2/guide/conditional.html#使用-key-控制元素的可重用)：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

在代码中切换 `loginType` 不会删除用户已经输入的内容，两个模版由于使用了相同的元素 `<input>` 会被复用，仅仅是替换了他们的 `placeholder`。

<br>

所以 `Vue` 提供一种方式让你可以自己决定是否要复用元素。你要做的是添加一个属性 `key` ，`key` 必须带有唯一的值。

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

现在输入文本将会在每次切换时重新渲染。

<br>

### 三、v-show

`v-show` 和 `v-if` 用法上大体一样，不同的是有 `v-show` 的元素会始终渲染并保持在 DOM 中。`v-show` 是简单的切换元素的 CSS 属性 `display` 。

```html
<h1 v-show="ok">Hello!</h1>
```

> 注意：`v-show` 不支持 `<template>` 语法

##### `v-if` 和 `v-show` 比较:

`v-if` 是真实的条件渲染，因为它会确保条件块在切换当中适当地销毁与重建条件块内的事件监听器和子组件。

`v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。

相比之下， `v-show` 简单得多——元素始终被编译并保留，只是简单地基于 CSS 切换。

一般来说， `v-if` 有更高的切换消耗而 `v-show` 有更高的初始渲染消耗。因此，如果需要频繁切换使用 `v-show` 较好，如果在运行时条件不大可能改变则使用 `v-if` 较好。

<br>

#### 结语：本章的内容和官方文档大体上一样，因为没啥好补充的。平时 `v-show` 用的比较多。