<template>
  <div id="create">
    <notifications group="edit"/>
    <div class="d-flex">
      <h4 class="font-weight-bold">Edit current task</h4>
      <router-link to="/" class="btn btn-primary ml-auto">To list</router-link>
    </div>
    <hr />
    <form class="mt-2" method="post" @submit.prevent="edit">
      <div class="form-group">
        <p class="mr-5">Status: {{ this.status }}</p>
        <input v-model="task.complited" type="checkbox" id="status">
        <label for="status">Tap to change status</label>
      </div>
      <div class="form-group">
        <small>Title</small>
        <input v-model="task.title" type="text" class="form-control" required/>
      </div>
      <div class="form-group">
        <small>Description</small>
        <textarea v-model="task.description" class="form-control" rows="5" required/>
      </div>
        <input class="btn btn-success" type="submit" value="Edit task" />
    </form>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      task: "",
    }
  },
  created(){
    axios.get(`http://localhost:3000/tasks/${this.$route.params.id}`)
      .then(response => (this.task = response.data))
      .catch(error => console.error(error));
  },
  computed: {
    status() {
      if(this.task.complited){
        return "complited"
      }else{
        return "progress"
      }
    }
  },
  methods: {
    edit() {
      axios
        .patch(`http://localhost:3000/tasks/${this.$route.params.id}`, {
          title: this.task.title,
          description: this.task.description,
          complited: this.task.complited
        })
        .then(this.$notify({
          group: 'edit',
          title: 'Задача изменена',
          type: 'success',
          closeOnClick: true
        }))
        .catch(error => console.log(error))
        
    }
  }
}
</script>

<style lang="scss" scoped>
input[type=checkbox] {
  height: 20px;
  width: 20px;
  margin-right: 10px;
  &:hover {
    cursor: pointer;
  }
}
</style>
