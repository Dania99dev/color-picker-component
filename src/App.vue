<template>
  <h1>Color picker Component for vue</h1>
  <ColorPicker />
</template>

<script lang="ts">
import { defineComponent } from "vue";

import ColorPicker from "@/components/ColorPicker.vue";

export default defineComponent({
  name: "App",
  components: {
    ColorPicker
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
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>
