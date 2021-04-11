<template>
  <div id="create">
    <notifications group="create"/>
    <div class="d-flex">
      <h4 class="font-weight-bold">Add new task</h4>
      <router-link to="/" class="btn btn-primary ml-auto">To list</router-link>
    </div>
    <hr />
    <form class="mt-2" method="post" @submit.prevent="addTask">
      <div class="form-group">
        <small>Title</small>
        <input v-model="title" type="text" class="form-control" required/>
      </div>
      <div class="form-group">
        <small>Description</small>
        <textarea v-model="description" class="form-control" rows="5" required/>
      </div>
      <input class="btn btn-success" type="submit" value="Add task" />
    </form>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      title: "",
      description: ""
    }
  },
  methods: {
    clear(){
      this.title = "",
      this.description = ""
    },
    addTask() {
      axios
        .post("http://localhost:3000/tasks", {
          title: this.title,
          description: this.description,
          complited: false
        })
        .then(this.clear())
        .then(this.$notify({
          group: 'create',
          title: 'Задача добавлена',
          type: 'success',
          closeOnClick: true
        }))
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>

