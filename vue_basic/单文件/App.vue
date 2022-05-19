<template>
  <div>
    <school></school>
    <student></student>
  </div>
</template>

<script>
import School from './单文件/School.vue'
import Student from './单文件/Student.vue'
export default {
  name: 'App',
  components: { School, Student },

}
</script>

<style>

</style>