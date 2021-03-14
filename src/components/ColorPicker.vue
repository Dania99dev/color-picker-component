<template>
  <div class="container">
    <svg id="color-box" width="300" height="200">
      <defs>
        <linearGradient id="saturation" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop
            offset="0%"
            style="stop-color:rgb(255,255,255);stop-opacity:1"
          />
          <stop
            offset="100%"
            style="stop-color:rgb(255,255,255);stop-opacity:0"
          />
        </linearGradient>
        <linearGradient id="brightness" x1="0%" y1="100%" x2="0%" y2="0%">
          <stop offset="0%" style="stop-color:rgb(0,0,0);stop-opacity:1" />
          <stop offset="100%" style="stop-color:rgb(0,0,0);stop-opacity:0" />
        </linearGradient>
      </defs>
      <rect width="100%" height="100%" fill="url(#saturation)" />
      <rect width="100%" height="100%" fill="url(#brightness)" />
    </svg>
    <input type="range" name="hue" id="hue" />
    <p>Color picker</p>
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "ColorPicker"
});
</script>

<style scoped>
.container {
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
  padding: 1rem;
  border-radius: 0.5rem;
  display: flex;
  flex-direction: column;
}
#color-box {
  background-color: blue;
}
#hue {
  appearance: none;
  margin: 0.75rem 0;
}
</style>
