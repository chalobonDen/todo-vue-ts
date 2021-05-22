<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <!-- <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" /> -->
    <todolist></todolist>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
// import HelloWorld from "./components/HelloWorld.vue";
import Todolist from "./components/Todolist.vue";

@Component({
  components: {
    // HelloWorld,
    Todolist,
  },
})
export default class App extends Vue {}
</script>

<style lang="less">
@background: #4fc08d;
body {
  background-color: @background;
}
#app {
  background: rgb(255, 255, 255);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-right: 10px;
  margin-left: 10px;
}
img {
  width: 150px;
  height: 150px;
}
</style>
