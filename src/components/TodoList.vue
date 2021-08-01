
<template>
<el-table
    :data="tableData.filter(data => !search | data.name.toLowerCase().includes(search.toLowerCase()))"
    style="width: 100%">
    <el-table-column
      label="ID"
      prop="id">
    </el-table-column>
    <el-table-column
      label="name"
      prop="name">
    </el-table-column>
     <el-table-column
      label="category"
      prop="category">
    </el-table-column>
     <el-table-column
      label="participants"
      prop="participants">
    </el-table-column>
     <el-table-column
      label="status"
      prop="status">
    </el-table-column>
    <el-table-column
      align="right">
      <template #header>
        <el-input
          v-model="search"
          size="mini"
          placeholder="Type to search"/>
        <el-button 
          type="text" 
          @click="showCreateDialog = true">
            open a Form nested Dialog1
          </el-button>
      </template>
      <template #default="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
        <delete-todo-button :row="scope.row" v-on:todo-deleted="deleteRow(scope.$index)"/>
      </template>
    </el-table-column>
  </el-table>
  <el-dialog title="Create new Todo" v-model="showCreateDialog">
    <create-todo-form v-on:todo-created="addRow($event)"/>
  </el-dialog>
</template>

<script>
import CreateTodoForm from './CreateTodoForm.vue'
import DeleteTodoButton from './DeleteTodoButton.vue'
  export default {
  components: { DeleteTodoButton, CreateTodoForm },
      name: 'TodoList',
      props: {},
    data() {
      return {
        tableData: [],
        search: '',
        showCreateDialog: false,
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
        this.tableData = this.tableData.splice(index,1)
      },
      addRow(row){
        this.tableData.push(row)
      }
    }
  }
</script>
