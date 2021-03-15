<template>
  <div id="selected-color" :style="selectedColor"></div>
  <div class="container">
    <svg
      id="color-box"
      width="300"
      height="200"
      :style="{ backgroundColor: `hsl(${hue}, 100%, 50%)` }"
      @mousedown="isMouseDown = true"
      @mousemove="newMarkerPos"
      @mouseup="isMouseDown = false"
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
        id="marker"
        cx="50%"
        cy="50%"
        r="6"
        stroke="white"
        stroke-width="2"
        fill="transparent"
      />
    </svg>
    <input type="range" name="hue" id="hue" v-model="hue" min="0" max="360" />
    <p>Color picker</p>
    hue: {{ hue }}
    <br />
    saturation: {{ saturation }}
    <br />
    brightness: {{ brightness }}
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "ColorPicker",
  data() {
    return {
      hue: 180,
      saturation: 50,
      brightness: 25,
      isMouseDown: false
    };
  },
  methods: {
    newMarkerPos(e: any) {
      if (this.isMouseDown) {
        const colorBox = document.querySelector("#color-box");
        const marker = document.querySelector("#marker");
        if (colorBox !== null && marker !== null) {
          const clickX = (e.layerX / colorBox.clientWidth) * 100;
          const clickY = (e.layerY / colorBox.clientHeight) * 100;
          marker.attributes[1].value = `${clickX}%`;
          marker.attributes[2].value = `${clickY}%`;
          this.saturation = Math.round(clickX);
          this.brightness = Math.round(50 - clickY / 2);
        }
      }
    }
  },
  computed: {
    selectedColor: function(): object {
      return {
        backgroundColor: `hsl(${this.hue}, ${this.saturation}%, ${this.brightness}%)`
      };
    }
  }
});
</script>

<style scoped>
#selected-color {
  margin: 1rem;
  width: 50px;
  height: 50px;
}
.container {
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.05);
  padding: 1rem;
  margin: 1rem;
  border-radius: 0.5rem;
  display: flex;
  flex-direction: column;
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
  width: 14px;
  height: 14px;
  border-radius: 50%;
  border: 3px solid white;
  cursor: grab;
}
#hue::-webkit-slider-thumb:active {
  cursor: grabbing;
}
</style>
