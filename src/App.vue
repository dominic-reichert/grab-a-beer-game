<template>
  <HelloWorld />
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
};
</script>

<style>
*,
::before,
::after {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  margin: 0;
}

#app {
  display: grid;
  grid-template-rows: auto 1fr;

  height: 100%;

  font-family: monospace, sans-serif;
}
</style>
