
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
          type="primary" 
          @click="showCreateDialog = true">
            Create
          </el-button>
          <el-button
          size="primary"
          @click="showUpdateDialog = true">
            Edit
          </el-button>
      </template>
      <template #default="scope">
        <delete-todo-button :row="scope.row" v-on:todo-deleted="deleteRow(scope.$index)"/>
      </template>
    </el-table-column>
  </el-table>
  <el-dialog title="Create new Todo" v-model="showCreateDialog">
    <create-todo-form :isEdit="false" v-on:todo-created="handleOnCreated($event)" />
  </el-dialog>
  <el-dialog title="Update new Todo" v-model="showUpdateDialog">
    <create-todo-form :isEdit="true" v-on:todo-updated="handleOnUpdated($event)" />
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
        showUpdateDialog: false,
        isEdit: true
      }
    },
    mounted () {
      axios
        .get('http://localhost:8081/rest/tasks')
        .then(response => (this.tableData = response.data))
    },
    methods: {
      deleteRow(index){      
        this.tableData = this.tableData.splice(index,1)
      },
      handleOnCreated(row){
        this.tableData.push(row)
        this.showCreateDialog = false
      },
      handleOnUpdated(row){
        const hasSameId = (element) => element.id === row.id
        let index = this.tableData.findIndex(hasSameId);
        this.tableData[index] = row
        this.showUpdateDialog = false
      }
    }
  }
</script>
