<template>
Hello {{isEdit}}
  <el-form :model="form">
    <el-form-item v-if="isEdit" label="Id" :label-width="formLabelWidth">
      <el-input-number v-model="form.id" @change="loadById()"></el-input-number>
    </el-form-item>
    <el-form-item label="Category" :label-width="formLabelWidth">
    <el-select v-model="form.category" placeholder="Select">
      <el-option
        v-for="category in categories"
        :key="category.value"
        :label="category.label"
        :value="category.value">
      </el-option>
    </el-select>
    <el-button type="primary" :disabled="!form.category" @click="loadFromRandomApi()">{{todoElement}}<i class="el-icon-magic-stick"></i></el-button>
    </el-form-item>
    <el-form-item label="Name" :label-width="formLabelWidth">
      <el-input v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item label="Participants" :label-width="formLabelWidth">
      <el-input-number v-model="form.participants"></el-input-number>
    </el-form-item>
    <el-form-item v-if="isEdit" label="States" :label-width="formLabelWidth">
    <el-select v-model="form.status" placeholder="Select">
      <el-option
        v-for="status in states"
        :key="status"
        :label="status"
        :value="status">
      </el-option>
    </el-select>
    </el-form-item>
  </el-form>
    <el-button :disabled="isEdit && form.id == null | !isEdit" type="primary" @click="save()">Save</el-button>
</template>

<script>
  export default {
    name: 'CreateTodoForm',
    props: {
      isEdit: Boolean
    },
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
          id: null,
          name: '',
          category: '',
          participants: 0,
          status: null
        },
        formLabelWidth: '120px',
        states: []
      };
    },
    mounted () {
      axios
        .get('http://localhost:8081/rest/task/states')
        .then(response => (this.states = response.data))
    },
    methods: {
       loadById(){
        axios
            .get('http://localhost:8081/rest/task/' + this.form.id)
            .then(response => {
              this.form = response.data
              })
      },
      loadFromRandomApi(){
        axios
            .get('http://www.boredapi.com/api/activity?type=' + this.form.category)
            .then(response => {
              this.form.name = response.data.activity
              this.form.participants = response.data.participants
              })
      },
      resetForm(){
        this.form.id = 0
        this.form.name = ''
        this.form.category = ''
        this.form.status = 'OPEN'
        this.form.participants = 0 
      },
      save(){
        if(this.isEdit){
          axios
            .put('http://localhost:8081/rest/task/' + this.form.id, this.form)
            .then(response => {
              if(response.status === 200){this.$emit('todo-updated', response.data)}
              this.resetForm()
              })
        } else {
          axios
            .put('http://localhost:8081/rest/tasks', this.form)
            .then(response => {
              if(response.status === 201){this.$emit('todo-created', response.data)}
              this.resetForm()
              })
        }
        
      }
    }
  };
</script>