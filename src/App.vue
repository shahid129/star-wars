<template>
  <h1>{{ title }}</h1>
  <Character />
</template>
  
  <script>
import Character from "./components/Character.vue";

export default {
  name: "App",
  components: { Character },
  data() {
    return {
      title: "Star-War Heroes",
    };
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
  