<template>
  <div id="app">
    <mymap />
  </div>
</template>

<script>
import mymap from "./components/map.vue";

export default {
  name: "App",
  components: {
    mymap
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
