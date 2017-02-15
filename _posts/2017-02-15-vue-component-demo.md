---
title: Vue.js 组件简单实例
---

{% raw %}
```html
<div id="app">
    <div>
        <comp-name v-bind:arg1='para1' v-bind:arg2="para2"></comp-name>
        <!-- 
        然后这里就相当于函数调用。
        arg1,和para1的对应关系就相当于形参和实参的对应关系。
        至于para1具体的值来源于Vue对象的app.para1
        -->
    </div>

</div>
<script>
    Vue.component('comp-name', {
        props: ['arg1', 'arg2'],
        template: '<p>This is  {{ arg1 }} and {{ arg2 }}</p>'
    });
    /*
    * 某种意义上一个component有点像一个函数，
    * 那么这里comp-name是函数名，props是形参表，template是函数体
    */
    let app = new Vue({
        el: "#app",
        data: {
            para1: "you",
            para2: 'me'
        }
    })
</script>
```
{% endraw %}
