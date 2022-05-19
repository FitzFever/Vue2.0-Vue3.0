<template>
  <ul class="todo-main">
    <Item v-for="todoObj in todos" :todoObj="todoObj"></Item>
  </ul>
</template>

<script>
import Item from './Item.vue'
export default {
  components: { Item,},
  name:'List',
  data(){
    return {
      todos:[
        {id:'001',name:'吃饭',complete:true},
        {id:'002',name:'睡觉',complete:false},
        {id:'003',name:'打豆豆',complete:true},
      ]
    }
  }
}
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>