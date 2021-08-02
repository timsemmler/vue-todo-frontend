<template #default="scope">
    <el-button type="danger" icon="el-icon-delete" @click="handleClick()" circle></el-button>
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
            .then(response => {if(response.status === 204){this.$emit('todo-deleted',response.data)}})
        }
      }
  }
</script>