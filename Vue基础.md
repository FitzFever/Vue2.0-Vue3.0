# Vue基础

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

   ```heml
   /例子
   @keyup.ctrl.y = "showInfo" /当按下ctral+y时才会触发showInfo
   ```

 ## Vue计算属性

> 只要Vue实例中的`data`数据发生改变,`methods`下使用到`data`数据的方法就到用重新执行

**get**:底层是靠getter实现,所以仅在初次读取或者所以来的数据发生变化时才调用`get`

**set**:如果计算属性会被修改,那就需要在计算属性内部协商`set`方法 

