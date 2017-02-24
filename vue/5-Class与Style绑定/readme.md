Class与Style绑定
---

说在前面：

在传统的jquery开发中，我们通常是通过判断和操纵节点的 `class` 属性来做一些页面特效，但是这样的开发有时会非常的复杂和难以维护的。但是在Vue中，DOM是用数据驱动的，DOM可以随着数据的变化而变化，所以千万不要低估了Vue，100行的jquery代码，可能用Vue只要50行。当然这并不意味着Vue和jquery是互斥关系，一切看场景。class和style的绑定在平常的开发中是会经常用的，而且它们的用法也不会仅限于以下几种。

### 一、class绑定

#### 1.绑定一个数据变量或者计算属性

class是一个html标签的属性，既然是标签属性，那么就可以直接用 `v-bind` 绑定一个数据变量或者计算属性，例如：

```html
<style>
    .text-green {color: green;}
    .text-red {color: red;}
</style>

<div id="app">
    <h1 :class="myClass">this is message 1</h1>
    <input type="text" v-model="message">
    <h1 :class="textAlign">this is message 2</h1>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            myClass: 'text-green',
            message: ''
        },
        computed: {
            textAlign: function () {
                if (this.message.length % 2 === 0) {
                    return 'text-green'
                } else {
                    return 'text-red'
                }
            }
        }
    })
</script>
```
运行这个示例，第一个`h1`标签会一直是绿色的，第二个`h1`标签会根据输入框中字符串长度的改变而改变。

[演示](http://lavyun.github.io/learn-vue/vue/5-Class与Style绑定/demo1.html)

#### 2.绑定一个对象

```html
<div :class="{ active : index}"></div>
```

div是否应用类`active`取决于index是否为true。这句话怎么理解呢？我们把上面的例子用对象写法写一遍就知道了：

```html
<div id="app">
    <h1 :class="{ 'text-green': myClass}">this is message 1</h1>
    <input type="text" v-model="message">
    <h1 :class="{ 'text-green': textAlign, 'text-red': !textAlign }">this is message 2</h1>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            myClass: true,
            message: ''
        },
        computed: {
            textAlign: function () {
                if (this.message.length % 2 === 0) {
                    return true;
                } else {
                    return false;
                }
            }
        }
    })
</script>
```

得到的效果跟上面的是一样的。

> 注意：当`class`对象的属性是 `kebab-case`形式的 (短横线隔开式)，需要加上引号，如上面的`text-green`，如果是`textGreen`就不用加上引号了。事实上，在javascript里面，你最好在任何时候都对`kebab-case`形式的属性写法都加上引号。

#### 3.绑定一个数组

前面提到，class可以绑定一个data数据，但是如果我的class属性有多个值呢？难道data数据要这样写？

```html
<div :class="myClass"></div>
```
```javascript
data:{
    myClass: 'text-green background-red text-center'
}
```

如果我们的class是动态改变的，可能颜色是red,yellow......这样写的弊端就会体现出来。我们每改一次class都需要对整个`myClass`字符串进行更改，代码量会加大。所以Vue提供了class属性的数组绑定方法。

```html
<div :class="[text-color, brakground, align]"></div>
```
```javascript
data: {
    'text-color': 'text-green',
    'background': 'background-red',
    'align': 'text-center'
}
```

现在如果我们需要改颜色，只需要对`text-color`进行更改就好了。

<br>

### 二、Style绑定

#### 1.style绑定一个对象

`v-bind:style` 的对象语法十分直观——看着非常像 CSS ，其实它是一个 JavaScript 对象。 CSS 属性名可以用驼峰式（camelCase）或短横分隔命名（kebab-case）：

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```javascript
data: {
  activeColor: 'red',
  fontSize: 30
}
```

接下来我们做一个小练习，做一个简易的图片轮播效果，有三张图片和三个按钮，当鼠标移到第一个按钮上时，就切换到第一张图片,第二个按钮切换到第二张图片。。。

首先我们添加三张图片到images目录中，并把它们的名字添加到数组中：
```javascript
data: {
    imgList: ['bg1.png','bg2.jpg','bg3.jpg']
}
```

还有一个变量`index`，用来表示数组的索引，我们想只需要对`index`进行修改，轮播会观察`index`的变化来自己切换图片：
```javascript
data: {
    imgList: ['bg1.png','bg2.jpg','bg3.jpg'],
    index: 0
}
```

接下来我们来写div的style，注意这里的字符串拼接
```html
<div class="wrap" :style="{ 'background-image': 'url(../images/' + imgList[index] + ')' }"></div>
```

然后又三个按钮，绑定按钮的mouseover事件：
```html
<button @mouseover="index=0">1</button>
<button @mouseover="index=1">2</button>
<button @mouseover="index=2">3</button>
```

当我们鼠标移到某个按钮上时，`index`就会被修改，而`div`的`style`标签中有对`index`的`getter`，所以`index`改变时`style`也会改变，就达到了我们想要的效果

全部代码如下：
```html
<style>
    .wrap{width: 300px;height: 180px;background-size: 100% 100%;background-position: center;background-repeat: no-repeat}
</style>

<div id="app">
    <div class="wrap" :style="{ 'background-image': 'url(../images/' + imgList[index] + ')' }"></div>
    <button @mouseover="index=0">1</button>
    <button @mouseover="index=1">2</button>
    <button @mouseover="index=2">3</button>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            imgList: ['bg1.png','bg2.jpg','bg3.jpg'],
            index: 0
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/5-Class与Style绑定/demo3.html)

#### 2.style绑定一个数组
`v-bind:style` 的数组语法可以将多个样式对象应用到一个元素上：
```html
<div :style="[style1, style2]"></div>
```
```javascript
data: {
    style1: {
        width: '200px',
        height: '200px'
    },
    style2: {
        background: '#d4d4d4'
    }
}
```

#### 3.自动添加前缀

当 `v-bind:style` 使用需要特定前缀的 CSS 属性时，如 `transform` ，Vue.js 会自动侦测并添加相应的前缀。