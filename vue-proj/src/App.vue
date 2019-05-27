<template>
  <div id="app">
    <h3>{{ getFullName }}</h3>
    Link: <a :href="linkToGoogle">Google</a>
    <div
      class="status"
      :class="{active:isStatus}"
    >{{ isStatus }}</div>
    <input
      type="text"
      v-model="text"
    >
    <p>{{ text }}</p>
    <p>{{ calculate(15, 119)}}</p>

    <button @click="popup('Hello')">Alert</button>

    <h4 v-if="score >= 50">Pass</h4>
    <h4 v-if="score < 50">Fail</h4>

    <ul>
      <li
        v-for="skill in skills"
        :key="skill.name"
      >
        Name : {{ skill.name }} &emsp;
        Skill : {{ skill.skill }}
      </li>
    </ul>

    <StudentInfo
      :firstName="'Kanจ้า  '"
      :lastName="'Lowongsa'"
    />
    <iframe
      src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3787.609442394985!2d99.39550051537789!3d18.31946048007401!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x30d96f16d7b2b1d9%3A0xc878b2cf01890b8a!2zVW5uYW1lZCBSb2FkLCDguJXguLPguJrguKUg4Lib4LiH4Lii4Liy4LiH4LiE4LiBIOC4reC4s-C5gOC4oOC4rSDguKvguYnguLLguIfguInguLHguJXguKMg4Lil4Liz4Lib4Liy4LiHIDUyMTkw!5e0!3m2!1sth!2sth!4v1558793676072!5m2!1sth!2sth"
      width="600"
      height="450"
      frameborder="0"
      style="border:0"
      allowfullscreen
    ></iframe>
  </div>
</template>

<script>
import StudentInfo from "./components/StudentInfo.vue";

export default {
  name: "app",

  data() {
    return {
      firstName: "Kan",
      lastName: "Lowongsa",
      linkToGoogle: "https://www.google.com",
      isStatus: true,
      text: "",
      score: 50,
      skills: [{ name: "Art", skill: "PHP" }, { name: "Kan", skill: "Java" }]
    };
  },

  computed: {
    getFullName() {
      return "I'm " + this.firstName + "  " + this.lastName;
    }
  },

  methods: {
    calculate: function(num1, num2) {
      return num1 + num2;
    },

    popup: function(msg) {
      alert(msg);
    }
  },

  components: {
    StudentInfo
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

.status {
  background-color: red;
  color: rgb(255, 253, 253);
}

.active {
  background-color: green;
  color: #000;
}
</style>
