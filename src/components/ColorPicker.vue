<template>
  <div id="selected-color" :style="selectedColor"></div>
  <div class="container">
    <svg
      id="color-box"
      width="250"
      height="200"
      :style="{ backgroundColor: `hsl(${hsb.h}, 100%, 50%)` }"
      @mousedown="isMouseDown = true"
      @mousemove="newMarkerPos"
      @mouseup="isMouseDown = false"
      @touchstart="(isMouseDown = true), newMarkerPos"
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
        :cx="`${hsb.s}%`"
        :cy="`${100 - hsb.b}%`"
        r="6"
        stroke="white"
        stroke-width="2"
        fill="transparent"
      />
      <circle
        id="marker"
        :cx="`${hsb.s}%`"
        :cy="`${100 - hsb.b}%`"
        r="4"
        stroke="black"
        stroke-width="2"
        fill="transparent"
      />
    </svg>
    <input type="range" name="hue" id="hue" v-model="hsb.h" min="0" max="360" />
    <div class="color-inputs">
      <div class="color-prop hue-number-input">
        H<input type="number" v-model="hsb.h" min="0" max="360" />
      </div>
      <div class="color-prop saturation-number-input">
        S<input type="number" v-model="hsb.s" min="0" max="100" />
      </div>
      <div class="color-prop brightness-number-input">
        B<input type="number" v-model="hsb.b" min="0" max="100" />
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "ColorPicker",
  data() {
    return {
      hsb: {
        h: 360,
        s: 100,
        b: 100
      },
      hsl: {
        h: 0,
        s: 100,
        l: 50
      },
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
          this.hsb.s = Math.round(clickX);
          this.hsb.b = Math.round(100 - clickY);
        }
      }
    }
  },
  computed: {
    selectedColor: function(): object {
      return {
        backgroundColor: `hsl(${this.hsb.h}, ${this.computedSaturation *
          100}%, ${this.computedLightness * 100}%)`
      };
    },
    computedLightness: function(): number {
      return parseFloat(
        ((this.hsb.b / 100) * (1 - this.hsb.s / 200)).toFixed(2)
      );
    },
    computedSaturation: function(): number {
      if (this.computedLightness === 0 || this.computedLightness === 1) {
        return 0;
      } else {
        return (
          (this.hsb.b / 100 - this.computedLightness) /
          Math.min(this.computedLightness, 1 - this.computedLightness)
        );
      }
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
  font-size: 0.875rem;
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
