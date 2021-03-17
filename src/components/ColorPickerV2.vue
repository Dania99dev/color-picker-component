// ! line 9 should be fixed after setting conversion functions
<template>
  <div class="container">
    <svg
      id="color-box"
      :width="colorBoxWidth"
      :height="colorBoxHeight"
      :style="{
        backgroundColor: `hsl(${selectedColor.hsb.h}, 100%, 50%)`
      }"
    >
      <defs>
        <linearGradient id="saturation" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" style="stop-color:rgba(255,255,255,1)" />
          <stop offset="100%" style="stop-color:rgba(255,255,255,0)" />
        </linearGradient>
        <linearGradient id="brightness" x1="0%" y1="100%" x2="0%" y2="0%">
          <stop offset="0%" style="stop-color:rgba(0,0,0,1)" />
          <stop offset="100%" style="stop-color:rgba(0,0,0,0)" />
        </linearGradient>
      </defs>
      <rect width="100%" height="100%" fill="url(#saturation)" />
      <rect width="100%" height="100%" fill="url(#brightness)" />
      <circle
        id="white-marker"
        :cx="`${selectedColor.hsb.s}%`"
        :cy="`${100 - selectedColor.hsb.b}%`"
        r="6"
        stroke="white"
        stroke-width="2"
        fill="transparent"
      />
      <circle
        id="black-marker"
        :cx="`${selectedColor.hsb.s}%`"
        :cy="`${100 - selectedColor.hsb.b}%`"
        r="4"
        stroke="black"
        stroke-width="2"
        fill="transparent"
      />
    </svg>
    <input
      type="range"
      id="hue"
      min="0"
      max="360"
      v-model="selectedColor.hsb.h"
    />
    <div class="color-inputs">
      <div class="color-prop">
        <p>H</p>
        <input type="number" v-model="selectedColor.hsb.h" min="0" max="360" />
      </div>
      <div class="color-prop">
        <p>S</p>
        <input type="number" v-model="selectedColor.hsb.s" min="0" max="100" />
      </div>
      <div class="color-prop">
        <p>B</p>
        <input type="number" v-model="selectedColor.hsb.b" min="0" max="100" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "ColorPickerV2",
  props: {
    colorBoxWidth: {
      type: Number,
      reqired: true
    },
    colorBoxHeight: {
      type: Number,
      reqired: true
    }
  },
  data() {
    return {
      selectedColor: {
        hsb: {
          h: 360,
          s: 100,
          b: 100
        },
        hsl: {
          h: 360,
          s: 100,
          l: 100
        },
        rgb: {
          r: 255,
          g: 255,
          b: 255
        },
        hex: {
          r: "ff",
          g: "ff",
          b: "ff"
        }
      }
    };
  }
});
</script>

<style scoped>
.container {
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.05);
  border-radius: 0.5rem;
  padding: 1rem;
  display: flex;
  flex-direction: column;
}
#color-box {
  background-color: red;
}
#hue {
  appearance: none;
  margin: 0.75rem 0;
  background: linear-gradient(
    to right,
    hsl(0, 100%, 50%),
    hsl(30, 100%, 50%),
    hsl(60, 100%, 50%),
    hsl(90, 100%, 50%),
    hsl(120, 100%, 50%),
    hsl(150, 100%, 50%),
    hsl(180, 100%, 50%),
    hsl(210, 100%, 50%),
    hsl(240, 100%, 50%),
    hsl(270, 100%, 50%),
    hsl(300, 100%, 50%),
    hsl(330, 100%, 50%),
    hsl(360, 100%, 50%)
  );
  border-radius: 4rem;
}
#hue:focus {
  outline: none;
}
#hue::-webkit-slider-thumb {
  appearance: none;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  border: 2px solid white;
  cursor: grab;
}
#hue::-webkit-slider-thumb:active {
  cursor: grabbing;
}
.color-inputs {
  display: flex;
  justify-content: space-between;
}
.color-prop {
  display: flex;
  align-items: center;
}
.color-prop input {
  width: 3rem;
  padding: 0.5rem;
  margin: 0 0.25rem;
  border: 1px solid #949494;
  border-radius: 0.25rem;
}
.color-prop input:focus {
  outline: none;
}
.color-prop input::-webkit-inner-spin-button {
  appearance: none;
}
</style>
