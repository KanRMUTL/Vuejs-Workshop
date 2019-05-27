<template>
  <div>
    {{ fullName }}
  </div>
</template>

<script>
export default {
  name: "StudentInfo",

  props: ["firstName", "lastName"],

  computed: {
    fullName() {
      return this.firstName + this.lastName;
    }
  }
};
</script>

