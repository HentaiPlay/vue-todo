<template>
  <li class="d-flex">
    <input type="checkbox" @click="onChangeStatus" :checked="task.complited" />
    <span :class="{ linethrough: task.complited }">{{ task.title }}</span>
    <router-link :to="`/edit/${this.task.id}`" class="ml-auto">
      <span class="material-icons bg-success ">create</span>
    </router-link>
    <span @click="onDelete" class="material-icons bg-danger ml-2">
      delete_outline
    </span>
  </li>
</template>

<script>
import axios from "axios"
export default {
  props: ['task'],
  methods: {
    onChangeStatus(){
      this.task.complited = !this.task.complited
      this.$emit('onChangeStatus', { 
        status: this.task.complited 
      })
      axios
        .patch(`http://localhost:3000/tasks/${this.task.id}`, { 
          complited: this.task.complited 
        })
        .catch(error => {
          console.log(error)
        })
    },
    onDelete(){
      this.$emit('onDelete', {id: this.task.id})
    }
  }
};
</script>

<style lang="scss" scoped>
li {
  border: 1px solid grey;
  border-radius: 15px;
  padding: 10px;
  margin-top: 10px;
  align-items: center;
  input {
    height: 20px;
    width: 20px;
    margin-right: 10px;
    &:hover {
      cursor: pointer;
    }
  }
  .material-icons {
    color: #fff;
    padding: 8px;
    border-radius: 50%;
    &:hover {
      cursor: pointer;
    }
  }
  .linethrough{
    text-decoration: line-through;
  }
}
</style>