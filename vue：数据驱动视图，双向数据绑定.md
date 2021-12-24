vue：数据驱动视图，双向数据绑定

MVVM：Model数据源，View视图，ViewModel vue的实例

调试工具：vue-devTool

创建vue实例

```js
const v=new vue({
	el:'#app',//指向页面的一个element*区域*，后面接一个*选择器*
	data:{    //数据
		username:'zss',
		info:'<h1>hahaha</h1>',
        count:0,
        flag:true,
        list:[
            {id:1,name:'ls'},
            {id:2,name:'ww'}
        ]
	},
	methods:{  //定义事件的处理函数
		/*add:function () {
            v.count+=1;
        }*/
        add() {
            this.count+=1  //this指向v，v是vue对象，count是它的属性
		}
	}
})
```

指令：vue模板语法

1、**内容渲染**

​	`v-text`

```html
<p v-text="username">xxx</p> //username会覆盖xxx
```

​	`{{}}`  //插值表达式，除了绑定简单的数值外，还支持js表达式运算

```html
<p>{{username}}</p>
<p>{{1+2}}</p>
<p>{{1 ? yes : no}}</p>
<p>{{username.split('').reverse().join('')}}</p>
```

​	`v-html`  //解决前两种只能渲染纯文本的缺陷，可以将标签渲染成html内容

```html
<div v-html="info"></div>
```

2、**属性绑定**

​	`v-bind` //给属性动态绑定值,可以简写成 `：`

```html
	<input type="text" v-bind:placeholder="username"></input>
=》
	<input type="text" :placeholder="username"></input>

<!--如果属性绑定需要用到动态拼接，字符串外面需要包裹''-->
<div :title="'姓名是'+username"></div>
```

3、**事件绑定**

​	`v-on`  对应原生DOM时间onClick，onKeyUp等，简写：`@`

```html
	<button v-on:click="add">+1</button>
=》
	<button @click="add">+1</button>
```

​	`vue提供了内置变量，名字叫做 $event , 相当于原生DOM的事件对象 e`

​	事件修饰符

> ​	.prevent 阻止默认行为
>
> ​	.stop 阻止事件冒泡
>
> ​	.capture 以捕获模式触发当前事件处理函数
>
> ​	.once 绑定的事件只触发一次
>
> ​	.self 只有在event.target是当前元素自身时触发事件处理函数

```html
<a href="">跳转</a>
原生阻止：e.preventDefault()阻止默认事件
		e.stopPropagation()阻止冒泡
vue 中 :prevent事件修饰符
<a href="" @click.prevent="xxx">跳转</a>
```

​	按键修饰符

> 判断按下的具体键
>
> .enter  按下enter键
>
> .esc .....

```html
<input type="text" @keyup.enter="show"/>
```

4、**双向绑定**

> 不操作DOM的情况下，拿到表单的数据

```html
<input type="text" v-model="username"/>//自动监听更换data中数据
```

​	input输入框、textarea、select可以使用v-model,其他没有意义

​	v-model指令的修饰符

> ​	.number   自动将用户输入的字符转为数字
>
> ```
> <input type="text" v-model.number="age"/>
> ```
>
> ​	.trim  自动去除用户输入字符串首尾的空格
>
> ```
> <input type="text" v-model.trim="username"/>
> ```
>
> ​	.lazy  在change时而非input时更新data数据
>
> ```
> <input type="text" v-model.lazy="username"/>
> ```

5、**条件渲染**

​	`v-if`        动态添加或移除DOM

```html
<p v-if="flag">v-if控制</p>
```

​	`v-show`   相当于动态为元素添加或移除 `display:none`样式

```html
<p v-show="flag">v-show控制</p>
```

> ​	如果要频繁切换，使用v-show性能更好；如果后期很可能不需要被展示，使用v-if性能更好

​	`v-else-if`    `只能在v-if后面使用，不然会检测失败`

```
<p v-if="1">优秀</p>
<p v-else-if="2">良好</p>
<p v-else>一般</p>
```

6、**列表渲染**

​	`v-for`

```html
<table>
    <thead>
        <!--<th>索引</th>-->
        <th>id</th>
        <th>姓名</th>
    </thead>
    <tbody>
        <!--只要用到v-for一定要绑定一个:key属性，key的值只能是数字或字符串，且具有唯一性-->
        <tr v-for="item in list" :key="item.id">
        <!--<tr v-for="(item,index) in list">  需要索引的话可以加上 index -->
        <!--<td>{{index}}</td>-->
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
        </tr>
    </tbody>
</table>
```

> 用index来作为key值没有意义，因为随着页面的重新渲染, key 值也发生了变化，和原来的数据没法建立关联关系，就失去了key的意义，可能导致数据紊乱。

过滤器（私有过滤和全局过滤）、vue3去掉了，必须有return

**侦听器**（1.方法格式：简单，不能立即触发，如果侦听的是对象，对象属性的变化不会触发  2.对象格式：需要立即触发时使用，immediate：true；**深度监听**：需要侦听对象属性时使用，deep：true）

​	不用深度监听而用方法侦听对象的子属性：‘info.username’（单引号不能丢）

**计算属性**

```html
data：{
	r:0,
	g:0,
	b:0
}
动态更换背景颜色
<div class="box" :style="{background:`rgb($(r),$(g),$(b))`}">
    <!--使用模板字符串-->
    {{`rgb($(r),$(g),$(b))`}}
</div>

使用计算属性
使用vue实例中computed节点,所有计算属性都要定义到此节点之下
computed:{
//rgb作为一个计算属性被定义成了方法，需要返回一个生成好的rgb
	rgb(){
		return `rgb($(this.r),$(this.g),$(this.b))`
	}
}
使用后：
<!--可以直接使用计算属性中的rgb,当普通属性来用-->
<div class="box" :style="{background:rgb}">
    {{rgb}}
</div>
```

​	优点：

​	1.实现了代码的复用

​	2.只要计算属性中依赖的数据源变了，则计算属性

会自动重新求值