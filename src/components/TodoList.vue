
<template>
<el-table
    :data="tableData.filter(data => !search | data.name.toLowerCase().includes(search.toLowerCase()) | data.status.toLowerCase().includes(search.toLowerCase()))"
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
        <span>
        <el-input v-model="search" size="mini" placeholder="Type to search"/>
        <el-button type="primary" icon="el-icon-plus" @click="openCreateDialog()" circle></el-button>  
        </span>
      </template>
      <template #default="scope">
        <delete-todo-button :row="scope.row" v-on:todo-deleted="deleteRow(scope.$index)"/>
        <el-button type="primary" icon="el-icon-edit" @click="openEditDialog(scope.$index)" circle></el-button>
      </template>
    </el-table-column>
  </el-table>
  <todo-dialog :elementToEdit="selectedElement" :show="isDialogOpen" v-on:todo-created="handleOnCreated($event)" v-on:todo-updated="handleOnUpdated($event)"/>
</template>

<script>
import TodoDialog from './TodoDialog.vue'
import DeleteTodoButton from './DeleteTodoButton.vue'

  export default {
  components: { DeleteTodoButton, TodoDialog },
      name: 'TodoList',
      props: {
      },
    data() {
      return {
        tableData: [],
        search: '',
        isDialogOpen: false, 
        isEdit: true,
        selectedElement: {}
      }
    },
    mounted () {
      axios
        .get('http://localhost:8081/rest/tasks')
        .then(response => {this.tableData = response.data}
        )
    },
    methods: {
      openEditDialog(index){
        this.selectedElement = this.tableData[index]
        this.isDialogOpen = true
      },
      openCreateDialog(){
        this.selectedElement = null
        this.isDialogOpen = true
      },
      deleteRow(index){      
        this.tableData = this.tableData.splice(index,1)
      },
      handleOnCreated(row){
        this.tableData.push(row)
        this.isDialogOpen = false
      },
      handleOnUpdated(row){
        const hasSameId = (element) => element.id === row.id
        let index = this.tableData.findIndex(hasSameId);
        this.tableData[index] = row
        this.isDialogOpen = false
      }
    }
  }
</script>
