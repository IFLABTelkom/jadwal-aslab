<template>
  <div
    id="app"
    class="hero is-dark is-fullheight"
  >
    <Schedule />
  </div>
</template>

<script>
import Schedule from "./components/Schedule.vue";
export default {
  name: "app",
  components: {
    Schedule
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  overflow: hidden;
}
</style>
