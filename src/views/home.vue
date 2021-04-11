<template>
  <div id="todolist">
    <notifications group="home"/>
    <div class="d-flex">
      <h4 class="font-weight-bold">Task list:</h4>
      <router-link to="/create" class="btn btn-primary ml-auto">
        Add new task
      </router-link>
    </div>
    <hr />
    <ul v-if="tasks.length > 0">
      <vselect 
        @changeSelect="changeSelect"
      />
      <hr />
      <itemList
        v-for="task of selectedTask" 
        :key="task.id"
        :task="task"
        :selectedTask="selectedTask"
        @onChangeStatus="onChangeStatus"
        @onDelete="onDelete"
      />
    </ul>
    <div v-else><span>No tasks found :(</span></div>
  </div>
</template>

<script>
import itemList from "@/components/itemList.vue"
import vselect from "@/components/select.vue"
import axios from 'axios';

export default {
  components: {
    vselect,
    itemList
  },
  data() {
    return {
      currentSelect: "all",
      id: '',
      tasks: []
    }
  },
  mounted(){
    this.getData()
  },
  computed: {
    selectedTask() {
      switch (this.currentSelect) {
        case "all":
          return this.tasks
        case "progress":
          return this.tasks.filter(el => el.complited == false)
        case "finished":
          return this.tasks.filter(el => el.complited == true)
        default:
          return this.tasks
      }
    }
  },
  methods: {
    changeSelect(payload){
      this.currentSelect = payload.select
    },
    onChangeStatus(payload){
      this.tasks.complited = payload.status
      this.$notify({
        group: 'home',
        title: 'Статус изменен',
        type: 'success',
        closeOnClick: true
      });
    },
    getData(){
      axios.get('http://localhost:3000/tasks')
        .then(response => (this.tasks = response.data))
        .catch(error => console.error(error))
    },
    async onDelete(payload){
      await (this.id = payload.id)
      await axios
        .delete(`http://localhost:3000/tasks/${this.id}`)
        .catch(error => console.error(error))
      this.getData()
      await this.$notify({
        group: 'home',
        title: 'Задача удалена',
        type: 'success',
        closeOnClick: true
      });
    }
  }
};
</script>

<style lang="scss" scoped>
ul {
  padding: 0;
  margin: 0;
}
</style>
