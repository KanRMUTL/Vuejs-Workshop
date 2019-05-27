<template>
  <div id="app">
    <child-test
      v-bind:parent-message="massage"
      v-on:childToParent="triggerChildFromParent"
    />
    <h2>{{ msgFromChild }}</h2>
  </div>
</template>

<script>
import ChildTest from "./components/ChildTest.vue";

export default {
  name: "app",

  data() {
    return {
      massage: " Hi my Child!!",
      msgFromChild: ""
    };
  },

  methods: {
    triggerChildFromParent: function(valFromChild) {
      this.msgFromChild = valFromChild;
    }
  },

  mounted() {
    this.$root.$on("ChildToRoot", () => {
      alert("Hey lv3");
    });
  },

  components: {
    ChildTest
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
</style>
