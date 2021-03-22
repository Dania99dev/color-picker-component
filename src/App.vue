<template>
  <div id="app-container">
    <h1 :style="{ color: defaultColorRGBstring }">
      ColorPicker Component Vue.js
    </h1>
    <div class="color-picker-container">
      <div
        id="selected-color"
        @click="colorPickerVisibility = !colorPickerVisibility"
        :style="{ backgroundColor: defaultColorRGBstring }"
      ></div>
      <div id="color-picker" v-if="colorPickerVisibility">
        <ColorPicker
          :colorBoxWidth="250"
          :colorBoxHeight="200"
          :defaultColor="defaultColor"
          @colorChange="onColorChange"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import ColorPicker from "@/components/ColorPicker.vue";
import { ColorGroup, HSB, RGB } from "@/types";

export default defineComponent({
  name: "App",
  components: {
    ColorPicker
  },
  data() {
    return {
      colorPickerVisibility: false,
      defaultColor: {
        h: Math.round(Math.random() * 360),
        s: 60,
        b: 70
      } as HSB,
      defaultColorRGBstring: "",
      selectedColor: {} as ColorGroup
    };
  },
  methods: {
    defaultColorRGBtoString(rgb: RGB): string {
      return `rgb(${rgb.r}, ${rgb.g}, ${rgb.b})`;
    },
    onColorChange(newColor: ColorGroup) {
      this.selectedColor = newColor;
      this.defaultColorRGBstring = this.defaultColorRGBtoString(
        this.selectedColor.rgb
      );
    }
  },
  beforeMount() {
    this.colorPickerVisibility = true;
  },
  mounted() {
    this.colorPickerVisibility = false;
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
  padding: 3rem;
}
.color-picker-container {
  position: relative;
  margin: 1rem;
}
#selected-color {
  width: 50px;
  height: 50px;
  position: relative;
}
#color-picker {
  position: absolute;
  left: 110%;
  top: 10%;
}
</style>
