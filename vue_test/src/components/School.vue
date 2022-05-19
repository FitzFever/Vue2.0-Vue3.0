<template>
  <div>
    <h2>{{name}}</h2>
    <h2>{{address}}</h2>
    <button @click="showName">name</button>
  </div>
</template>

<script>
import { mixin } from '../mixin'

export default {
  name: 'School',
  data(){
    return {
      name: 'balala',
      address: 'moxianbao',
    }
  },
  mixins:[mixin]
}
</script>

<style>

</style>