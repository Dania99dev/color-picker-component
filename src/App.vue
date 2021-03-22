<template>
  <div id="app-container">
    <h1 :style="{ color: selectedColorRGBstring }">
      ColorPicker Component Vue.js
    </h1>
    <div class="color-picker-container">
      <div
        id="selected-color"
        @click="colorPickerVisibility = !colorPickerVisibility"
        :style="{ backgroundColor: selectedColorRGBstring }"
      ></div>
      <div id="color-picker" v-show="colorPickerVisibility">
        <ColorPicker
          :colorBoxWidth="250"
          :colorBoxHeight="200"
          :defaultColor="defaultColor"
          @colorChange="newColor => (this.selectedColor = newColor)"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ColorPicker from "@/components/ColorPicker.vue";
import { ColorGroup, HSB, HSL, RGB, HEX } from "@/types";

export default defineComponent({
  name: "App",
  components: {
    ColorPicker
  },
  data() {
    return {
      colorPickerVisibility: false,
      defaultColor: {
        h: 345,
        s: 80,
        b: 90
      } as HSB,
      selectedColor: {
        hsb: {} as HSB,
        hsl: {} as HSL,
        rgb: {} as RGB,
        hex: {} as HEX
      } as ColorGroup
    };
  },
  computed: {
    selectedColorRGBstring(): string {
      return `rgb(${this.selectedColor.rgb.r}, ${this.selectedColor.rgb.g}, ${this.selectedColor.rgb.b})`;
    }
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
}
</style>
