<template>
  <div>
    <div>
      Hey I'm Children!! อิอิ <br>
      {{ parentMessage }}
      <button v-on:click="handleClick">Click me</button>
      <child-to-root />
    </div>
  </div>
</template>

<script>
import ChildToRoot from "./ChildToRoot.vue";
export default {
  name: "ChildTest",

  props: ["parentMessage"],

  data() {
    return {
      msgToParent: "Hey mom I hungry!!"
    };
  },

  methods: {
    handleClick: function() {
      this.$emit("childToParent", this.msgToParent);
    }
  },

  components: {
    ChildToRoot
  }
};
</script>

<style>
</style>

