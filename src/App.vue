<template>
  <div id="app" style="margin-top: 0">
    <MyMap />
  </div>
</template>

<script>
import MyMap from "./components/MyMap.vue";

export default {
  name: "App",
  components: {
    MyMap,
  },
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
