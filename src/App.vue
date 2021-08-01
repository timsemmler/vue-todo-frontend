<template>
  <el-container>
    <todo-list/>
  </el-container>
</template>

<script>
import TodoList from './components/TodoList.vue'
export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>
