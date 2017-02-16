### 模板语法
---


##### 一、{{ }}

Vue用双大括号的形式来绑定一个数据对象上的属性。

```html
<div id="app">
    {{ msg }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'hello world'
        }
    })
</script>

```

得到
```
hello world
```

{{ }} 绑定的是一个数据对象，可以是data，也可以是计算属性（后面将会提到）。

<br>

#### 二、html

Vue用`v-html`绑定一串 HTML

```html
<div id="app">
    <div v-html="someHTML">

    </div>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            someHTML:'<input type="text">'
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/3-模板语法/demo1.html)

此时页面上会出现一个文本框。

嘿嘿，想到好玩的了，我们可以用`v-html`和`v-model`来写一个简单的在线html编辑器。

先在页面上放一个`textarea`并且给它绑定一个`v-model`

```html
<div id="app">
    <textarea cols="150" rows="20" v-model="htmlSource"></textarea>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            htmlSource:'<div><img src="https://vuefe.cn/images/logo.png"></div>'
        }
    })
</script>
```

接下来我们把`v-model`绑定的数据传给一个`v-html`不就能解析出html代码了吗？完整代码如下：

```html
<div id="app">
    <textarea cols="150" rows="20" v-model="htmlSource"></textarea>
    <div v-html="htmlSource"></div>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            htmlSource:'<div><img src="https://vuefe.cn/images/logo.png"></div>'
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/3-模板语法/demo2.html)

输入一些html代码来看看效果吧。

<br>

#### 三、属性

Vue中用`v-bind`来绑定一个数据属性，我们来看看v-bind怎么用。

```html
<div id="app">
    <img v-bind:src="img">
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            img: "https://vuefe.cn/images/logo.png"
        }
    })
</script>
```
[演示](http://lavyun.github.io/learn-vue/vue/3-模板语法/demo3.html)

很明显，`v-bind:src`是将一个数据变量绑定到img标签的`src`属性上，也可以简写为`:src`。当数据变量改变时，html标签的属性也会随之改变。

又想到一个好玩的东西了，我要通过输入框来动态改变input标签的`type`属性，搞起来：

```html
<div id="app">
    <input type="text" v-model="inputType">
    <input :type="inputType">
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            inputType: 'text'
        }
    })
</script>
```

[演示](http://lavyun.github.io/learn-vue/vue/3-模板语法/demo4.html)

通过输入check,radio,file...来看看效果吧。

<br>

#### 四、过滤器

Vue允许你自定义过滤器，可被用作一些常见的文本格式化，由“管道”符表示。

```html
<div id="app">
    {{ num1 | isEven }}
    {{ num2 | isEven }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            num1: 1,
            num2: 2
        },
        filters:{
            isEven:function(value){ //判断是不是偶数的过滤器
                if(value%2==0){
                    return true;
                } else {
                    return false;
                }
            }
        }
    })
</script>
```
得到的结果是：
```
false true
```
[演示](http://lavyun.github.io/learn-vue/vue/3-模板语法/demo5.html)

<br>

过滤器也可以传递参数:
```
{{ message | filterA('arg1', arg2) }}
```

例如：
```html
<div id="app">
    {{ num1 | bigThan(4) }}
    {{ num2 | bigThan(1) }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            num1: 1,
            num2: 2
        },
        filters: {
            bigThan: function (value, num) { //判断是不是大于某个数的过滤器
                if (value > num) {
                    return true;
                } else {
                    return false;
                }
            }
        }
    })
</script>
```

得到结果为：
```
false true
```
[演示](http://lavyun.github.io/learn-vue/vue/3-模板语法/demo6.html)