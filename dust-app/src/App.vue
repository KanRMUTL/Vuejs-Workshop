<template>
  <div id="app">
   <location-list/>
  </div>
</template>

<script>
import locationList from "./components/locationList.vue";

export default {
  name: "app",
  components: {
    locationList
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  font-size: 3rem;
  color: #333;
}

p {
  font-size: 1.3rem;
  margin-top: -2rem;
}
</style>
