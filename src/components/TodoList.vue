
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
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">Delete</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
  export default {
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
        .catch(console.log('CATCH'))

      console.log(this.tableData)
    },
    methods: {
      handleEdit(index, row) {
        console.log(index, row);
      },
      handleDelete(index, row) {
        console.log(index, row);
      }
    },
  }
</script>
