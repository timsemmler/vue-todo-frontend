<template #default="scope">
    <el-button
          size="mini"
          type="danger"
          @click="handleClick()">Delete</el-button>
</template>

<script>
  export default {
    name: 'DeleteTodoButton',
    props: {
        row: Object
    },
    emits: ['todo-deleted'] ,
    methods: {
      handleClick() {
        axios
            .delete('http://localhost:8081/rest/task/' + this.row.id)
            .then(response => {if(response.status === 204){this.$emit('todo-deleted')})
        },
    }
  }
</script>