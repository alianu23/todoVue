<template>
  <HomePage></HomePage>
</template>

<script>
import HomePage from "./src/components/HomePage.vue";

export default {
  name: "App",
  components: {
    HomePage,
  },
};
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
  background-color: black;
  display: flex;
  justify-content: center;
  align-items: start;
}
</style>
