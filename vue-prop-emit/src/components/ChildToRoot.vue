<template>
  <div>
    Hey I'm Chile level 3.000
    <button v-on:click="handleClick">Emit To Root</button>
  </div>
</template>

<script>
export default {
  name: "ChildToRoot",

  methods: {
    handleClick: function() {
      this.$root.$emit("ChildToRoot");
    }
  }

};
</script>

