
<template>
<el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      label="ID"
      prop="id">
    </el-table-column>
    <el-table-column
      label="Bezeichnung"
      prop="bezeichnung">
    </el-table-column>
     <el-table-column
      label="Status"
      prop="status">
    </el-table-column>
    <el-table-column
      align="right">
      <template #header>
        <el-input
          v-model="search"
          size="mini"
          placeholder="Type to search"/>
      </template>
      <template #default="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
        <delete-todo-button :row="scope.row" v-on:todo-deleted="deleteRow(scope.$index)"/>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import DeleteTodoButton from './DeleteTodoButton.vue'
  export default {
  components: { DeleteTodoButton },
      name: 'TodoList',
      props: {},
    data() {
      return {
        tableData: [],
        search: '',
      }
    },
    mounted () {
      axios
        .get('http://localhost:8081/rest/tasks')
        .then(response => (this.tableData = response.data))
    },
    methods: {
      handleEdit(index, row) {
        console.log(index, row);
      },
      deleteRow(index){
        console.log("Try to delete Data with index" , index)
        this.tableData = this.tableData.splice(index,1)
      }
    },
  }
</script>
