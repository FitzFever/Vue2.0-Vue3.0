# Vue基础

## 数据代理

定义:通过一个对象代理对零一个对象中属性的操作(读/写)

```
let obj1 = {x:100}
let obj2 = {y:200}

//通过数据代理obj2的getter和setter来改变obj1里的值
Object.defineProperty(obj2, 'x', {
	get(){
		return obj1.x
	,}
	set(value){
		obj1.x = value
	}
})
```

![Vue中的数据代理](D:/my_code_house/Vue2.0-Vue3.0/Vue%E5%9F%BA%E7%A1%80.assets/Vue%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E4%BB%A3%E7%90%86.png)

![Vue中的数据代理_示意图](D:/my_code_house/Vue2.0-Vue3.0/Vue%E5%9F%BA%E7%A1%80.assets/Vue%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E4%BB%A3%E7%90%86_%E7%A4%BA%E6%84%8F%E5%9B%BE.png)

## Vue事件

### 事件修饰符

1. ` .prevent`阻止默认事件(常用)
2. `.stop`阻止事件冒泡 (常用)
3. `.once`是兼职触发一次(常用)
4. `.capture` 使用事件的捕获模式,即事件的处理在捕获阶段(默认是在冒泡阶段)
5. ` .self`只有`event.target`是当前元素才触发事件
6. `.passive` 事件的默认行为为立即执行,无序等待时间回调执行完毕

### 键盘事件

> Vue中常见的按键别名:enter\delete\esc\tab(必须n配合keydown)\space\up\down\left\right

其中crtl\alt\win\shift都是系统修饰键,需要在按下的同时按下其他键,随后释放其他键,事件才会被触发

### 小技巧

1. 如果需要多个修饰符,修饰符可以连着写

2. 如果需要用到系统修饰符+其他的键,也可以连着写.

   ```html
   //例子
   @keyup.ctrl.y = "showInfo" //当按下ctral+y时才会触发showInfo
   ```

 ## Vue计算属性

> 只要Vue实例中的`data`数据发生改变,`methods`下使用到`data`数据的方法就到用重新执行

**get**:底层是靠getter实现,所以仅在初次读取或者所以来的数据发生变化时才调用`get`

**set**:如果计算属性会被修改,那就需要在计算属性内部协商`set`方法 

## Vue监视属性

>  如果数据修改过后对页面未产生印象,则开发者工具不会记录

> @click里也可以写一些简单的语句,但**建议** 只写一句

### watch

如果在创建实例时就明确要监视什么就如下写

```vue
watch:{
	immediate:true, //初始化时让handler调用一下
	isHot:{ 
		handler(newValue, oldValue){//当`data`或者`computed`里的isHot被修改时执行这里的代码,}
	}
}
```

如果创建实例时不明确则:

```vue
vm.$watch('isHot',{
	immediate:true, //初始化时让handler调用一下
	isHot:{ 
		handler(newValue, oldValue){//当`data`或者`computed`里的isHot被修改时执行这里的代码,}
	}
})
```

### 深度监视

Vue自身可以检测对象內部值的改变,但Vue中的`watch`默认不检测对象内部值的改变,所以需要额外配置`deep:true`来监测到内部值的改变

```vue
watch:{
	//监视多级结构中的某个属性变化
	'numbers.a':{ //检测data里的numbers对象的a属性,因为是对象的属性,所以要以字符串的形式来写
		handler(newValue, oldValue){ }
	}
	
	//监视多级结构中所有属性的变化
	numbers:{
		deep:true,
	}
}
```

### 监视的语法糖

如果不需要配置`immediate`和`deep`只需要`handler`的时候就可以采用语法糖

```
watch:{
	isHot(newValue, oldValue){
		//原handler下的内容
	}
}
```

```
vm.$watch('isHot', function(newValue, oldValue){
	//原handler下的内容
})
```

### `watch ` 和 `computed` 的比较 

- `computed`能完成的, `watch` 也能完成

- 但如果需求中有需要进行**异步**计算,则只能使用**`watch`**, `computed`是实现不了异步.

两个小原则:

- 所有被Vue管理的函数,最好写成普通函数, 这样this的指向才是vm或 组件实例对象
- 所有不被Vue管理的函数（如:定时器的回调函数
- ajax的回调函数、Promise的回调函数等）, 最好写成箭头函数, 这样this的指向才是vm 或者 组件实例对象

## Vue样式绑定

### 字符串写法

```
<div :class="mood"></div>

........

new Vue({
	data:{
		mood: 'className'
	}
})
```

适用于：样式的类名不确定，需要动态绑定

### 数组写法

```
<div :class="arr"></div>

........

new Vue({
	data:{
		arr: ['a1', 'a2', 'a3']
	},
	methods:{
		click(){
			//对样式数组进行操作
		}
	}
})
```

适用于要绑定的样式个数不确定、名字也不确定

### 对象写法

适用于个数确定、名字也确定，但要动态决定用不用

### 绑定style样式

用对象的写法

```
<div :style="styleObj"></div>
...
...
...
new Vue({
	data:{
		styleObj:{
			fontSitz: '40px' //原始css样式时font-size 因此这里要用驼峰的形式
		}
	}
})
```

数组+对象

```
<div :style="styleArr"></div>
...
...
...
new Vue({
	data:{
		styleArr:['styleObj','styleObj2'],
		styleObj:{
			fontSitz: '40px' //原始css样式时font-size 因此这里要用驼峰的形式
		},
		styleObj2:{
		......
		}
	}
})
```

## Vue条件渲染

 ### v-show和v-if

切换频率高用`v-show`，因为`v-if` 是对DOM进行添加和删除节点

#### v-else-if

也有`v-else` 且这些判读之间从必须紧紧相挨,否则会被打断

### v-if与\<template>

`template`不影响DOM和结构,所以可以对批量依靠同一个`v-if`判断条件的标签使用`<template v-if="">`进行包裹

## Vue列表渲染

### v-for

`v-for="(value, index) in arr"` 或者 `v-for="(value, key) of obj"`

### key

使用`v-for`进行遍历生成标签时,最好要加上`:key=""`

> ![image-20220517120259660](D:/my_code_house/Vue2.0-Vue3.0/Vue%E5%9F%BA%E7%A1%80.assets/%E9%9D%A2%E8%AF%95%E9%A2%98_key%E7%9A%84%E4%BD%9C%E7%94%A8.png)

### 列表过滤

通过计算属性来过滤

```
computed:{
	filPersons(){
		return this.persons.filter((p) = > {
			return p.name.indexOf(this.keyWord) !== -1
		})
	}
}
```

### 列表排序

核心思想:**先过滤再排序**

```
computed:{
	filPersons(){
		//不着急return 暂存过滤结果
		const arr = this.persons.filter((p) = > {
			return p.name.indexOf(this.keyWord) !== -1
		})
		
		//判断一下是否需要排序
		if(排序条件){
			arr.sort((p1, p2) => {
				return //p2-p1是降序,p1-p2是升序(即return一个大于0的则交换,)
			})
		}
		return arr
	}
}
```

## Vue监视数据的原理

![Vue监视数据的原理](D:/my_code_house/Vue2.0-Vue3.0/Vue%E5%9F%BA%E7%A1%80.assets/Vue%E7%9B%91%E8%A7%86%E6%95%B0%E6%8D%AE%E7%9A%84%E5%8E%9F%E7%90%86-16529311940885.png)

1. vue会监视`data`中所有层次的数据。

2. 如何监测对象中的数据？
   通过`setter`实现监视，且要在`new Vue`时就传入要监测的数据。

   1. 对象中后追加的属性，`Vue`默认不做响应式处理
   2. 如需给后添加的属性做响应式，请使用如下API:
      `Vue.set(target,propertyName/index,value)`
      或
      `vm.$set(target,propertyName/index,value)`
   3. 如何监测数组中的数据？
      通过包裹数组更新元素的方法实现，本质就是做了两件事：
      1. 调用原生对应的方法对数组进行更新。
      2. 重新解析模板，进而更新页面。
   4. 在Vue修改数组中的某个元素一定要用如下方法：
      1. 使用这些API:`push()、pop()、shift()、unshift()、splice()、sort(、reverse()`
      2. `Vue.set()`和`vm.$set()`

   特别注意：`Vue.set()`和`vm.$set()`不能给`vm`或`vm`的根数据对象添加属性！！！

## 收集表单数据

- 若：`<input type:="text"/>`,则v-model收集的是value值，用户输入的就是value值。
- 若：`<input type="radio"/>`,则v-modelr收集的是value值，且要给标签配置value值。
- 若：`<input type="checkbox"/>`
  - 1,没有配置input的value属性，那么收集的就是checked(勾选or未勾选，是布尔值)
  - 2,配置input的value属性：
    - (1)v-model的初始值是非数组，那么收集的就是checked(勾选or未勾选，是布尔值)
    - (2)v-model的初始值是数组，那么收集的的就是value组成的数组
- 备注：v-model的三个修饰符：
  - lazy:失去焦点再收集数据
  - number:输入字符串转为有效的数字
  - trim:输入首尾空格过滤

## 过滤器

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）

语法：

1. 注册过滤器：`Vue.filter(name,cal1back)`(全局注册)或`new Vue(filters:()}`(局部注册)
2. 使用过滤器：`{{xxx|过滤器名}}` 或 `v-bind:属性="xxx|过滤器名"`

备注：

1. 过滤器也可以接收额外参数、多个过滤器也可以串联
2. 并没有改变原本的数据，是产生新的对应的数据

## 常见内置指令

> v-bind:・单向绑定解析式，・可简写：XXX
> v-model→：·双向数据绑定
> v-for,→：·遍历数组/对象/字符串
> v-on·→：·绑定件监听，可简写为@
> V-if→·→：·条件渲染（动态控制节点是否存存在）
> v-else·:条件渲染（动态控制节点是否存存在）
> v-show
> 条件渲染（动态控制节点是否展示）

### v-text

想其所在的节点中渲染文本

### v-html

相较于`v-text`可以解析所有的结点 

1. 作用：向指定节点中渲染包含html结构的内容。
2. 与插值语法的区别：
   1. `v-html`会替换掉节点中所有的内容，{{xx}}则不会。
   2. `v-html`可以识别html结构。
   3. 严重注意：`v-html`有安全性问题！！！！
      1. 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
      2. 一定要在可信的内容上使用`v-html`,永不要用在用户提交的内容上！

### v-cloak

### v-once

### v-pre

1. 跳过其所在节点的编译过程.
2. 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。

## Vue自定义指令

Vue模板语法有2大类：

1. 插值语法：
   	功能：用于解析标签体内容。
   	写法：`{{xxx}}` , `xxx`是js表达式，且可以直接读取到data中的所有属性。

2. 指令语法：

   ​	功能：用于解析标签（包括：标签属性、标签体内容、绑定事件......）。
   ​	举例：`v-bind:href="xxx"`或简写为`：href="xxx"`,`xxx`同样要写js表达式，
   且可以直接读取到data中的所有属性。
   ​	备注：Vue中有很多的指令，且形式都是：v-???,此处我们只是拿`v-bind`举个例子。

自定义指令总结：

1. 定义语法：

   1. 局部指令：

      ``` 
      new Vue({
      	directives:(指令名：配置对象)
      })
      ```

      或

      ``` 
      new Vue({
      	directives.(指令名：回调函数)
      })
      ```

      

   2. 全局指令：
      `vue.directive(指令名，配置对象)`或
      `vue.directive(指令名，回调函数)`

2. 配置对象中常用的3个回调：

   1. `bind:`指令与元素成功绑定时调用。
   2. `inserted:`指令所在元素被插入页面时调用。
   3. `update:`指令所在模板结构被重新解析时调用。

3. 备注：

   1. 指令定义时不加`v-`,但使用时要加`v-`:
   2. 指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名。

## 生命周期

![](D:/my_code_house/Vue2.0-Vue3.0/Vue%E5%9F%BA%E7%A1%80.assets/Vue%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

## 组件

Vue中使用组件的三大步骤：
一、定义组件（创建组件）
二、注册组件
三、使用组件（写组件标签）
一、
如何定义一个组件？
使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options儿乎一样，但也有点区别：
区别如下：
1.e1不要写，为什么？一
最终所有的组件都要经过一个vm的管理，由vm中的e1决定服务哪个容器。
2,data必须写成函数，为什么？
避免组件被复用时，数据存在引用关系。
备注：使用template可以配置组件结构。
二、
如何注册组件？
I,局部注册：靠new Vue的时候传入components选项
2,全局注册：靠Vue.component('组件名'，组件)
三、编写组件标签：

### 组件注意点

1. 关于组件名：

​	一个单词组成：
​		第一种写法（首字母小写）：`school`
​		第二种写法（首字母大写）：`School`
​	多个单词组成：
​		第一种写法(kebab-case命名)：`my-school`
​		第二种写法(CamelCase命名)：`MySchool`(需要Vue脚手架支持)

​	备注：

1. 组件名尽可能回避HTML中己有的元素名称，例如：`h2`、`H2`都不行。

2. 可以使用`name`配置项指定组件在**开发者工具**中呈现的名字。

2. 关于组件标签：
   	第一种写法：`<school></school>`
   第二种写法：`<schoo1/>`
   备注：不用使用脚手架时，`<schoo1/>`会导致后续组件不能渲染。

3. 一个简写方式：
   `const school=Vue.extend(options)`可简写为：`const school=options`

### VueComponents构造函数

关于VueComponent:

1. `school`组件本质是一个名为`VueComponent`的构造函数，且不是程序员定义的，是`Vue.extend`生成的。
2. 我们只需要写`<schoo1/>`或`<school></schoo1>`, Vue解析时会帮我们创建`school`组件的实例对象，
   即Vue帮我们执行的：`new VueComponent(options)`.
3. 特别注意：每次调用`vue.extend`,返回的都是一个全新的`VueComponent`!!!!只不过都长得一模一样
4. 关于this指向：
   1. 组件配置中：
      data函数、methods中的函数、watch中的函数、computed中的函数它们的this均是【`VueComponent`实例对象】
   2. `new Vue()`配置中：
      `data`函数、`methods`中的函数、`watch`中的函数、computed中的函数它们的this均是【`Vue`实例对象】。
5. `VueComponent`的实例对象，以后简称vc(也可称之为：组件实例对象)。
   `Vue`的实例对象，以后简称vm。

​		vm可以指定`el`, 但是vc不行

#### `VueComponent` 的与一个重要内置关系

`VueComponent.prototype.__proto__ === Vue.prototype`即`VueComponent`的原型是`Vue`的实例对象

![image-20220519103429159](D:/my_code_house/Vue2.0-Vue3.0/Vue%E5%9F%BA%E7%A1%80.assets/image-20220519103429159.png)

从而让组件实例对象(vc)可以访问到 Vue原型上的属性和方法

### 单文件组件

