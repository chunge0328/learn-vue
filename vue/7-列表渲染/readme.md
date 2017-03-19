列表渲染
----

### 一、v-for

Vue用 `v-for` 指令对数组或对象进行渲染：

#### 1.数组迭代

使用 `v-for` 对数组各项元素进行迭代：

```html
<p v-for="item in girls">{{ item }}</p>
```
```javascript
data: {
    girls: ['cendy', 'tony', 'nancy']
}
```

结果：

```
cendy
tony
nancy
```

> 注意： 这里的 `item` 是迭代的别名，你完全可以用其它的：

```
<p v-for="girl in igirls">{{ girl }}</p>
```

这样也是没问题的，不过怎么看着怪怪的。>_>）

<br>

`v-for` 还支持一个可选的第二个参数为当前项的索引：

```html
<p v-for="(girl, index） in girls">
    No. {{ index }} : {{girl}}
</p>
```
```javascript
data: {
     girls: ['cendy', 'tony', 'nancy']
}
```

结果为：

```
No.1 : cendy
No.2 : tony
No.3 : nancy
```

<br>

#### 2.对象迭代

也可以用 `v-for` 对对象进行迭代，此时可以有第二个参数表示键名，第三个参数表示索引：

```html
<p v-for="(value, key, index) in object">
    {{index}} - {{key}} : {{value}}
</p>
```
```javascript
data: {
    object: {
        name: 'nancy',
        age: 18
    }
}
```

结果：

```
0 - name : nancy
1 - age : 18
```

<br>

#### 3.整数迭代

`v-for` 也可以取整数。在这种情况下，它将重复多次模板：

```
<div>
  <span v-for="n in 10">{{ n }}</span>
</div>
```

结果：

```
1 2 3 4 5 6 7 8 9 10
```

<br>

下面我们来做个小练习，在平常的网站中都有排行榜，排行榜的前几名都会应用不同的样式，比如字体颜色不一样，icon不一样，那怎么在Vue的列表循环中给某一项做特殊的处理呢？

我们思考一下，什么可以用来唯一的表示迭代的某一项，对，就是 `index` 参数，善用 `v-for` 的 `index` 会给你带来很多方便。

我们可以把 `index` 绑定到元素的 `class` 属性上去，再对某个特殊的`class`应用某个特殊的样式：

```html
<style>
    .range{display: inline-block;width: 20px;text-align: center;border-radius: 5px;color: #fff}
    .range-0{background-color: red}
    .range-1{background-color: blueviolet}
    .range-2{background-color: green}
</style>

<div id="app">
    Music Ranking list
    <p v-for="(item, index) in items">
       <span :class="'range-' + index" class="range">{{index}}</span> {{item}}
    </p>
</div>
```

```javascript
new Vue({
    el: '#app',
    data: {
        items: ['北京欢迎你', '上海欢迎你', '杭州欢迎你', '深圳欢迎你', '成都欢迎你']
    }
})
```

[演示](http://lavyun.github.io/learn-vue/vue/7-列表渲染/demo1.html)

但是这样做，每个列表项都会被设置形如 `range-(index)` 的类名，尽管有些`class`是没用的，所以我们可以改进下：

```html
<span :class="{['range-' + index]: index < 3}" class="range">{{index}}</span> {{item}}
```

这样只有前三项会被应用`range-(index)`。

<br>

### 二、数组的变异方法

##### 1.变异方法与非变异方法
什么是数组的变异方法？就是那些会改变原数组结构的数组方法。比如：

```javascript
arr.push(1)  //arr的长度增加了，并且新增了一个元素，所以是变异方法
[2,3,1].sort() //数组结构发生改变，变异方法
```

常见的数组变异方法有：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

> 注意： 变异方法会触发视图的更新

<br>

有变异方法，当然就有非变异方法，非变异方法就是不会改变原始数组，但总是返回一个新数组。比如：

```javascript
var old = [1, 2, 3];
var newArr = old.map(function (value) {
    return 2 * value;
})
console.log(old) //[1, 2, 3]
console.log(newArr) //[2, 4, 6]
```

`map` 方法产生一个新数组，原数组没有改变，所以是非变异方法。常见的非变异方法有：

- map()
- filter()
- slice()
- every()
- some()
- ......

<br>

##### 2.不会触发视图更新的操作

当直接用索引设置一个项时，不会触发视图更新，可以用Vue.set()来解决：

```html
<p v-for="item in items">{{ item }}</p>
<button @click="changeIndex">直接改变索引</button>
<button @click="VueSet">Vue.set()</button>
```

```javascript
data: {
    items: [1, 2, 3, 4, 5]
},
methods: {
    changeIndex: function () {
        this.items[1] = new Date();
    },
    VueSet: function () {
        Vue.set(this.items, 1, new Date());
    }
}
```

点击第一个按钮时没有任何效果，第二个按钮点击时可以改变 `this.items[1]`

[演示](http://lavyun.github.io/learn-vue/vue/7-列表渲染/demo2.html)


