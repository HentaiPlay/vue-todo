<template>
  <select @click="changeSelect" class="custom-select" id="select" v-model="select">
    <option value="all" selected>All</option>
    <option value="progress">Progress</option>
    <option value="finished">Complited</option>
  </select>
</template>

<script>
export default {
  data() {
    return {
      select: "all"
    }
  },
  methods: {
    changeSelect(){
      this.$emit('changeSelect', { select: this.select })
    }
  }
};
</script>

<style lang="scss" scoped>
#select {
  height: 50px;
  border-radius: 10px;
  border: 1px solid grey;
  &:hover {
    cursor: pointer;
  }
}
</style>