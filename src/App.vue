<template>
  <div id="app-container">
    <h1>ColorPicker Component vue.js</h1>
    <div class="color-picker-container">
      <div
        id="selected-color"
        @click="colorPickerVisibility = !colorPickerVisibility"
      ></div>
      <div id="color-picker" v-if="colorPickerVisibility">
        <ColorPickerV2 :colorBoxWidth="250" :colorBoxHeight="200" />
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
  },
  data() {
    return {
      colorPickerVisibility: false
    };
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
.color-picker-container {
  position: relative;
  margin: 1rem;
}
#selected-color {
  width: 50px;
  height: 50px;
  background-color: crimson;
  position: relative;
}
#color-picker {
  position: absolute;
  left: 110%;
  top: 10%;
}
</style>
