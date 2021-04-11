<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-8 offset-2">
          <h2 class="text-center font-weight-bold m-5">To Do</h2>
          <router-view />
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Ubuntu&display=swap");

body {
  font-family: "Ubuntu", sans-serif;
  margin: 0;
  padding: 0;
}
</style>
