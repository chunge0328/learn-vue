### Hello Vue

---

什么都别说了，赶紧来写个hello world吧。

```html
<script src="../lib/vue.min.js"></script>

<body>
    <div id="app">
        hello {{msg}}
    </div>
</body>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'vue'
        }
    })
</script>
```

得到如下结果
```
hello vue
```


<br><br>
再绑定一个用户事件

```html
<div id="app">
    hello {{msg}}
    <button v-on:click="handleClick">click</button>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Vue'
        },
        methods: {
            handleClick: function () {
                this.msg = 'everybody';
            }
        }
    })
</script>
```
[示例](http://lavyun.github.io/learn-vue/vue/2-HelloVue/demo1.html)

这里的`v-on：click`是绑定点击事件的意思，只要是合法的浏览器事件都可以绑定，也可以简写为`@click`，当点击按钮时`hello vue` 就变成了`hello everybody`

<br><br>
再来看一个双向绑定
```html
<div id="app">
    <input type="text" v-model="msg">
    {{msg}}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg:'hello vue'
        }
    })
</script>
```
[示例](http://lavyun.github.io/learn-vue/vue/2-HelloVue/demo2.html)

当改变输入框中的值的时候，绑定的数据也会随之改变，这个效果的实现后面会提到。