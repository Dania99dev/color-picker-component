<template>
  <div class="container">
    <svg id="color-box" :width="colorBoxWidth" :height="colorBoxHeight">
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
        cx="50%"
        cy="50%"
        r="6"
        stroke="white"
        stroke-width="2"
        fill="transparent"
      />
      <circle
        id="black-marker"
        cx="50%"
        cy="50%"
        r="4"
        stroke="black"
        stroke-width="2"
        fill="transparent"
      />
    </svg>
    <input type="range" id="hue" min="0" max="360" />
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
</style>
