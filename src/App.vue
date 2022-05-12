<template>
  <div class="container">
    <Content />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Content from "./components/Content.vue";

export default defineComponent({
  name: "App",
  components: {
    Content,
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  max-width: 1280px;
  width: 100%;
  margin: auto;
}
</style>
