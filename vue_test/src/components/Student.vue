<template>
  <div>
    <h2>name:{{name}}</h2>
    <h2>age:{{age}}</h2>
    <button @click="showName">name</button>
  </div>
</template>

<script>
import {mixin} from '../mixin'

export default {
  name: 'Student',
  data(){
    return {
      name: 'Tom',
      age: '18'
    }
  },
  mixins:[mixin]
}
</script>

<style>

</style>