<template>
  <el-form :model="form">
    <el-form-item label="Category" :label-width="formLabelWidth">
    <el-select v-model="form.category" placeholder="Select">
      <el-option
        v-for="category in categories"
        :key="category.value"
        :label="category.label"
        :value="category.value">
      </el-option>
    </el-select>
    <el-button type="primary" :disabled="!form.category" @click="loadFromRandomApi()">Generate Random <i class="el-icon-magic-stick"></i></el-button>
    </el-form-item>
    <el-form-item label="Name" :label-width="formLabelWidth">
      <el-input v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item label="Participants" :label-width="formLabelWidth">
      <el-input-number v-model="form.participants"></el-input-number>
    </el-form-item>
  </el-form>
    <el-button type="primary" @click="create()" :justify="end">Save</el-button>
</template>

<script>
  export default {
    name: 'CreateTodoForm',
    props: {},
    data() {
      return {
        categories: [{
          value: 'education',
          label: 'Education'
        }, {
          value: 'recreational',
          label: 'Recreational'
        }, {
          value: 'social',
          label: 'Social'
        }, {
          value: 'diy',
          label: 'DIY'
        }, {
          value: 'charity',
          label: 'Charity'
        }, {
          value: 'cooking',
          label: 'Cooking'
        }, {
          value: 'relaxation',
          label: 'Relaxation'
        }, {
          value: 'music',
          label: 'Music'
        }, {
          value: 'busywork',
          label: 'Busywork'
        }],
        form: {
          name: '',
          category: '',
          participants: 0
        },
        formLabelWidth: '120px'
      };
    },
    methods: {
      loadFromRandomApi(){
        axios
            .get('http://www.boredapi.com/api/activity?type=' + this.form.category)
            .then(response => {
              console.log(response) 
              this.form.name = response.data.activity
              this.form.participants = response.data.participants
              })
      },
       create(){
        axios
            .put('http://localhost:8081/rest/tasks', this.form)
            .then(response => {
              console.log(response) 
              if(response.status === 201){this.$emit('todo-created', response.data)}
              })
      }
    }
  };
</script>