<template>
  <div id="app">
    <img
      v-if="statusLight == 'off'"
      src="./assets/off.png"
    >
    <img
      v-if="statusLight == 'on'"
      src="./assets/on.png"
    >

    <button v-on:click="handleSwtich">Click me pls</button>

    <table>
      <tr>
        <th>Who?</th>
        <th>Message</th>
        <th>Status</th>
      </tr>
      <tr
        v-for="light in lights"
        v-bind:key="light.User"
      >
        <td>{{ light.User }} - {{ light.Name }}</td>
        <td>{{ light.Message }}</td>
        <td>{{ light.Status }}</td>
      </tr>
    </table>

  </div>

</template>

<script>
import Axios from "axios";

export default {
  name: "app",

  data() {
    return {
      statusLight: "off",
      lights: []
    };
  },

  methods: {
    initLight: function() {
      Axios.get("/ligthstatus")
        .then(response => {
          console.log(response.data);
          this.statusLight = response.data.Status;
        })
        .catch(error => {
          console.log(error.response.data);
        });

      Axios.get("/ligth")
        .then(response => {
          console.log(response.data);
          this.lights = response.data;
        })
        .catch(error => {
          console.log(error.response.data);
        });
    },

    handleSwtich: function() {
      var tmpStatus = this.statusLight == "off" ? 'on' : 'off'
      Axios.post("/ligth", {
        User: "017",
        Name: "กานต์ครับอิอิ",
        Message: "🙂🙂🤩🙂🙂",
        Status: tmpStatus
      });
    }
  },

  mounted() {
    this.$nextTick(function() {
      window.setInterval(() => {
        this.initLight();
      }, 5000);
    });
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
