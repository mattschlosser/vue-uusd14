<template>
  <div id="app">
    <img alt="Vue logo" src="https://vuejs.org/images/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <form
      v-if="!hasLoggedIn"
      method="post"
      action="http://localhost:3333/login"
      @submit.prevent="login"
    >
      <input type="email" name="email" placeholder="Username" />
      <input type="password" name="password" placeholder="Password" />
      <button>Submit</button>
    </form>
    <div v-else>
      You are now logged in
      <li v-for="radio in radios" :key="radio.id">
        {{ radio.id }}
      </li>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  data() {
    return {
      hasLoggedIn: false,
      radios: [],
    };
  },
  created() {
    this.getRadios();
  },
  methods: {
    async getRadios() {
      this.radios = await fetch('http://localhost:3333/radios', {
        credentials: 'include',
        method: 'GET',
      }).then((e) => e.json());
    },
    async login(event) {
      console.log(event);
      await fetch('http://localhost:3333/login', {
        headers: {
          'content-type': 'application/json',
        },
        credentials: 'include',
        method: 'POST',
        body: JSON.stringify({
          email: event.target.elements.email.value,
          password: event.target.elements.password.value,
        }),
      });
      this.hasLoggedIn = true;
      this.getRadios();
    },
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
