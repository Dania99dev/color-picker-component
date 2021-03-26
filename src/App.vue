<template>
  <div id="app-container">
    <h1>
      ColorPicker Component Vue.js
    </h1>
    <ColorPicker colorName="primary" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ColorPicker from "@/components/ColorPicker.vue";

export default defineComponent({
  name: "App",
  components: {
    ColorPicker
  },
  data() {
    return {};
  }
});
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Helvetica, Arial,
    sans-serif, Apple Color Emoji, Segoe UI Emoji;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 3rem;
}
</style>
