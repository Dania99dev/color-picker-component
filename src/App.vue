<template>
  <div id="app-container">
    <h1>ColorPicker Component vue.js</h1>
    <div id="selected-color">
      <div id="color-picker">
        <ColorPickerV2 />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import ColorPickerV2 from "@/components/ColorPickerV2.vue";

export default defineComponent({
  name: "App",
  components: {
    ColorPickerV2
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
  min-height: 100vh;
  width: 100%;
  padding: 2rem;
}
#selected-color {
  width: 50px;
  height: 50px;
  background-color: crimson;
  margin: 1rem;
  position: relative;
}
#selected-color:hover {
  cursor: pointer;
}
#color-picker {
  position: absolute;
  left: 110%;
  top: 10%;
}
</style>
