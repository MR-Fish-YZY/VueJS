# vue指令：

v-if--------->控制dom插入移除（删除dom）

v-on----------->为dom绑定事件		简写：例------------->	@click="doSomething"

1. 函数参数---> v-on:click="say('参数')"

2. 事件修饰符

   写法：<a v-on:click.stop="doThis"></a>

   .stop	.prevent	.capture	.self	.once（只触发一次）	.passive（addEventListener中的passive）

3. 按键修饰符

   写法<input v-on:keyup.13="submit">（13为按键码）

   .enter	.tab	.delete（删除或退格键）	.esc	.space	.up	.down	.left	.right

   可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)：Vue.config.keyCodes.f1 = 112

4. 系统修饰键--->仅在按下相应按键时才触发鼠标或键盘事件的监听器。

   .ctrl	.alt	.shift	.meta

   <!-- Alt + C --> <input @keyup.alt.67="clear"> 

   <!-- Ctrl + Click --> <div @click.ctrl="doSomething">Do something</div>

   .exact修饰符--->精确控制触发

   <!-- 即使 Alt 或 Shift 被一同按下时也会触发 --> <button @click.ctrl="onClick">A</button> 

   <!-- 有且只有 Ctrl 被按下的时候才触发 --> <button @click.ctrl.exact="onCtrlClick">A</button>

    <!-- 没有任何系统修饰符被按下的时候才触发 --> <button @click.exact="onClick">A</button>

5. 鼠标按键修饰符

   .left	.right	.middle

v-show----------->控制显示隐藏（隐藏dom【display:none】）

v-bind----------->为dom绑定属性		简写：例-------------->	:href="url"

v-for----------->循环输出数据--->

1. 写法：'item in tiems'或'item of items'

2. 建议为每一项绑定key ---> v-bind:key='item.id' 或 :key='item.id'

3. 数组变异方法（改变原始数组）

   push()、pop()、shift()、unshift()、splice()、sort()、reverse()

4. 非变异方法（不改变原数组）

   filter()、concat()、slice()

5. **注意** 改变数组长度以及通过索引改变数组值 都不是响应式的！！！

   ​		为数组手动添加值也不是响应式的！！！

   ​	解决改变通过索引改变数组值--->使用vm.$set(vm.items, index, newValue)

   ​	解决修改数组长度--->使用vm.items.splice(newLength)

   ​	解决为数组添加值--->使用Vue.set(vm.items, propertyName, value) 或 vm.$set； 添加多个新属性 使用Object.assign()` 或 `_.extend()

   ​	vm.userProfile = Object.assign({}, vm.userProfile, {  age: 27,  favoriteColor: 'Vue Green' })

6. 显示过滤/排序后的结果

   ```js
   <li v-for="n in evenNumbers">{{ n }}</li>
   data: {
     numbers: [ 1, 2, 3, 4, 5 ]
   },
   computed: {
     evenNumbers: function () {
       return this.numbers.filter(function (number) {
         return number % 2 === 0
       })
     }
   }
   ```

   ```js
   <li v-for="n in even(numbers)">{{ n }}</li>
   data: {
     numbers: [ 1, 2, 3, 4, 5 ]
   },
   methods: {
     even: function (numbers) {
       return numbers.filter(function (number) {
         return number % 2 === 0
       })
     }
   }
   ```

7. v-for可以直接输出数字 <span v-for="n in 10">{{ n }} </span>

8. 同时渲染多个元素使用template包起来

9. v-if 和 v-for一起用时 可以分层

   <ul v-if="todos.length">  <li v-for="todo in todos">    {{ todo }}  </li> </ul>

10. 组件上使用v-for时 v-bind:key时必须的   

v-model----------->双向绑定

1. 可以用在text、textarea

   ​				checkbox、radio--->对应change事件

   ​				select

2. 修饰符

   .lazy--->由实时转为和change事件同步	

   .number--->自动转为数字，无法被parseFloat()解析 输出原值	

   .trim--->去除收尾空格

   ​			


v-html------------->解析html字符串（慎用）--->多用在富文本渲染

v-once--------------->只渲染一次

## 2.6.0新增用方括号括起来的 JavaScript 表达式作为一个指令的参数

*****[]内为字符串 null可以移除绑定 其他任何非字符串类型都会报错**

*****[]内引号和空格是无效的，可以用计算属性代替表达式**

*****[]内的字符串会被转为小写，实例中也必须是小写才能匹配**

例如，如果你的 Vue 实例有一个 `data` 属性 `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。

```
<a v-bind:[attributeName]="url"> ... </a>
<a v-on:[eventName]="doSomething"> ... </a>
```

当 `eventName` 的值为 `"focus"` 时，`v-on:[eventName]` 将等价于 `v-on:focus`。

## 修饰符

例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

<form v-on:submit.prevent="onSubmit"> </form>
# vue模板：

```javascript
  Vue.component('todo-item', {

​    props: ['todo'],

​    template: '<li>{{todo.text}}</li>'

  })
```

通过props进行父组件到子组件的数据传递 通过v-bind来接收数据

    <div id="app10">
        <ol>
            <todo-item v-for="item in dataList" v-bind:todo="item" v-bind:key="item.id">
            </todo-item>
        </ol>
    </div>



# vue生命周期

# ![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

*****不要在选项属性或回调上使用箭头函数**

# 数据绑定

{{ msg }}

{{}}内和vue指令内可以使用表达式（***语句不行！！！）【全局变量可以访问Math、Date；；；不可以访问自定义的全局变量】

```
{{ number + 1 }} 
{{ ok ? 'YES' : 'NO' }} 
{{ message.split('').reverse().join('') }} 
<div v-bind:id="'list-' + id"></div>
```

# 计算属性

模板内不应放入太多逻辑例如

<div id="example">   {{ message.split('').reverse().join('') }} </div>
对于任何复杂逻辑都应该使用计算属性computed

```
var vm = new Vue({  
	el: '#example',  
	data: {    message: 'Hello'  },  
	computed: {    // 计算属性的 getter    
		reversedMessage: function () {      // `this` 指向 vm 实例      
		return 	this.message.split('').reverse().join('')   
		}  
	} 
})
```

## 计算属性set方法

上述计算属性使用的是默认的get方法，也可以设置set方法当计算属性本身的值变化时，响应到其他值

```javascript
    var vm = new Vue({
        el: '#app12',
        data() {
            return {
                firstName: 'john',
                lastName: 'smith'
            }
        },
        computed: {
            fullName: {
                get: function(){
                    return this.firstName + ' ' + this.lastName
                },
                set: function(newValue){
                    console.log(newValue);
                    var name = newValue.split(' ');
                    this.firstName = name[0]
                    this.lastName = name[name.length - 1]
                }
            }
        },
    })
```

当设置vm.fullName = "steff nash" 则data中的数据也会变化

## 计算属性缓存VS方法

## 计算属性基于响应式依赖进行缓存 只有在依赖发生变化时才会重新求值（在computed中使用Date.now()不会实时刷新，因为Date.now()不是响应式依赖）

方法则每次触发渲染时就会执行函数

## 侦听属性/侦听器 watch/vm.$watch

watch回调是命令式的（复杂重复），计算属性是响应式的（简洁）

当需要在数据变化时执行异步或开销较大的操作时，侦听属性/侦听器的方式是最有用的！！！

处理一般数据用计算属性

# class与style绑定

## 对象写法



## 数组写法



## 计算属性写法



## 用在组件上写法

# vue事件

# 组件

