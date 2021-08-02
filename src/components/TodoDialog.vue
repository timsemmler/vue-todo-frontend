<template>
<el-dialog :title="title" v-model="show">
  <el-form :model="form" :rules="rules" ref="todoform">
    <el-form-item label="Category" :label-width="formLabelWidth" prop="category">
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
    <el-form-item label="Name" :label-width="formLabelWidth" prop="name">
      <el-input v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item label="Participants" :label-width="formLabelWidth" prop="participants">
      <el-input-number v-model="form.participants"></el-input-number>
    </el-form-item>
    <el-form-item v-if="this.elementToEdit" label="States" :label-width="formLabelWidth" prop="status">
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
  <template #footer>
    <span class="dialog-footer">
    <el-button type="primary" @click="save()">Save</el-button>
    </span>
  </template>
</el-dialog>
</template>

<script>
  export default {
    name: 'TodoDialog',
    props: {
      show:  Boolean,
      elementToEdit: Object
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
        rules: {
          category: [
            {required: true, message: 'Please select at an activity category', trigger: 'change' },
          ],
          name: [
            {required: true, message: 'Please input Activity name', trigger: 'blur' },
          ],
          participants: [
            {type:'number', required: true, message: 'Please input number of participants', trigger: 'change' },
          ],
          status: [
            {required: true, message: 'Please select the current state', trigger: 'change' },
          ]
        },
        form: {
          id: null,
          name: '',
          category: '',
          participants: 0,
          status: null
        },
        formLabelWidth: '120px',
        states: [],
        title: ""
      };
    },
    mounted () {
      axios
        .get('http://localhost:8081/rest/task/states')
        .then(response => (this.states = response.data))
    },
    watch: {
      show: {
        handler() {
          console.log("Watcher activated!!!", this.elementToEdit)
          if(this.elementToEdit) {
            this.form = this.elementToEdit
            this.title = "Edit Todo"
          } else {
            this.resetForm()
            this.title = "Create Todo"
          }          
        }
      }
    },
    methods: {
      loadFromRandomApi(){
        axios
            .get('http://www.boredapi.com/api/activity?type=' + this.form.category)
            .then(response => {
              this.form.name = response.data.activity
              this.form.participants = response.data.participants
              })
      },
      resetForm(){
        if( this.$refs['todoform']){
          this.$refs['todoform'].resetFields()
        }
      },
      save(){
        this.$refs['todoform'].validate(valid =>{
        if(valid){
          if(this.elementToEdit){
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
        })  
      }
    }
  };
</script>