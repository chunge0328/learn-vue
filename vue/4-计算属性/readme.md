计算属性
----

在上一节中我们提到了双括号表达式，其实Vue是可以在模板中进行表达式操作的，例如：
```html
<div id="app">
    {{ num > 0 ? true : false}}
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 2
        }
    })
</script>
```
这样会在页面上得到一个true。

> 注意：双大括号中只可以是一个变量或者是一个表达式，不可以是多个表达式，因为{{ message }}就相当于`return message;`，那么`{{num > 0 ? true : false}}`就相当于`return num > 0 ? true : false;`，return语句后面只能是一个表达式。

<br>

### 一、computed
但是这样过长的表达式会让模板难以维护，所以就有了计算属性，例如：

```html
<div id="app">
    {{ numBigThanZero }}
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 2
        },
        computed: {
            numBigThanZero: function () {
                return this.num > 0 ? true : false;
                //在计算属性中访问data中的数据需要加this
            }
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/4-计算属性/demo1.html)

计算属性，顾名思义，就是对数据进行计算的属性，计算属性可以很好的避免过长的表达式。

> 注意：计算属性默认只有getter，`numBigThanZero`的值依赖于`num`的值，当`num`改变时，`numBigThanZero`才会做出对应的计算并返回。如果你直接对计算属性进行`setter`操作是没有任何反应的，因为它默认只有`getter`

```html
<div id="app">
    {{ numBigThanZero }}
    <button @click="numBigThanZero=false">click</button>
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 2
        },
        computed: {
            numBigThanZero: function () {
                return this.num > 0 ? true : false;
            }
        }
    })
</script>
```

点击按钮后，`numBigThanZero`并没有变成`false`，控制台也不会有任何报错。

如果需要，计算属性也可以有一个setter。

```html
<div id="app">
    {{ numBigThanZero }}
    <button @click="numBigThanZero=false">click</button>
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 2
        },
        computed: {
            numBigThanZero: {
                get: function () {
                    return this.num > 0 ? true : false;
                },
                set: function (newValue) {
                    this.num = 0;
                }
            }
        }
    })
</script>
```

此时，点击按钮后,`numBigThanZero`就变成了false。

[演示](http://lavyun.github.io/learn-vue/vue/4-计算属性/demo2.html)

> 对`numBigThanZero`赋值后触发了setter函数，`num`发生了变化，`num`发生更新后又会返回来触发`numBigThanZero`的getter，所以返回了false。注意：setter是不能return的。

<br>

### 二、watch

watch用于观察 Vue 实例上的数据变动，当所观察的数据发生变动时，就会触发所定义的方法，例如：

```html
<div id="app">
    <input v-model="message">
    {{ messageLength }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message:'',
            messageLength:0
        },
        watch:{
            message:function(value){
                this.messageLength=value.length;
            }
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/4-计算属性/demo3.html)

有同学会问了，这用 `computed` 不是也可以实现吗？对的，但是我们想象一个极端的场景，例如：
计算一个字符串的开头字母、长度、元音字母个数、大写字母个数、数字个数......，用 `computed` 的话代码如下：

```html
<div id="app">
    <input type="text" v-model="message">
    {{ message }} 的首字母是 {{ a }}，长度是{{ b }}，其中有{{ c }}个元音字母，{{ d }}个大写字母，{{ e }}个数字
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: '',
            a: 0, b: 0, c: 0, d: 0, e: 0
        },
        computed:{
            a:function(){
                return this.message.substr(0,1);
            },
            b:function(){
                return this.message.length;
            },
            c:function(){
                return this.message.replace(/[^AaEeIiOoUu]/g,'').length;
            },
            d:function(){
                return this.message.replace(/[^A-Z]/g,'').length;
            },
            e:function(){
                return this.message.replace(/[^\d]/g,'').length;
            }
        }
    })
</script>
```
代码长而繁琐，但是如果用 `watch` 的话：
```html
<div id="app">
    <input type="text" v-model="message">
    {{ message }} 的首字母是 {{ a }}，长度是{{ b }}，其中有{{ c }}个元音字母，{{ d }}个大写字母，{{ e }}个数字
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            message: '',
            a: 0, b: 0, c: 0, d: 0, e: 0
        },
        watch: {
            message: function (value) {
                this.a = value.substr(0, 1);
                this.b = value.length;
                this.c = value.replace(/[^AaEeIiOoUu]/g, '').length;
                this.d = value.replace(/[^A-Z]/g, '').length;
                this.e = value.replace(/[^\d]/g, '').length;
            }
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/4-计算属性/demo4.html)

相对而言就简洁了不少，但是 `watch` 也有不好的地方，当页面初始化的时候，数据没发生变化，a,b,c,d,e的值都不会被计算出来。而 `computed` 却不关心这个。

<br>

#### 结语
> 一般情况下，`computed` 可以满足大部分需求，但并不意味着 `watch` 就没用了，`watch` 适用于一个数据更新引起多个数据更新的场景，而 `computed` 多个数据的更新导致一个数据发生更新的场景。还有当数据变化响应时，执行异步操作或开销较大的操作，`watch` 也是很有用的。例如[这个官方示例](https://vuefe.cn/v2/guide/computed.html#观察-Watchers)
