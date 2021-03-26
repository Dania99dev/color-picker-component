<template>
  <div class="container">
    <svg
      :id="`${colorName}-color-box`"
      :width="colorBoxWidth"
      :height="colorBoxHeight"
      :style="{
        backgroundColor: `hsl(${selectedColor.hsl.h}, 100%, 50%)`
      }"
      @mousedown="newMousePos"
    >
      <defs>
        <linearGradient
          :id="`${colorName}-color-saturation`"
          x1="0%"
          y1="0%"
          x2="100%"
          y2="0%"
        >
          <stop offset="0%" style="stop-color:rgba(255,255,255,1)" />
          <stop offset="100%" style="stop-color:rgba(255,255,255,0)" />
        </linearGradient>
        <linearGradient
          :id="`${colorName}-color-brightness`"
          x1="0%"
          y1="100%"
          x2="0%"
          y2="0%"
        >
          <stop offset="0%" style="stop-color:rgba(0,0,0,1)" />
          <stop offset="100%" style="stop-color:rgba(0,0,0,0)" />
        </linearGradient>
      </defs>
      <rect
        width="100%"
        height="100%"
        :fill="`url(#${colorName}-color-saturation)`"
      />
      <rect
        width="100%"
        height="100%"
        :fill="`url(#${colorName}-color-brightness)`"
      />
      <circle
        :cx="`${selectedColor.hsb.s}%`"
        :cy="`${100 - selectedColor.hsb.b}%`"
        r="6"
        stroke="white"
        stroke-width="2"
        fill="transparent"
      />
      <circle
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
    <p class="color-string">{{ selectedColorHEX.toUpperCase() }}</p>
    <p class="color-string">{{ selectedColorRGB }}</p>
    <p class="color-string">{{ selectedColorHSL }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { HSB, HSL, RGB, HEX, ColorGroup } from "@/types";

export default defineComponent({
  name: "ColorSelector",
  props: {
    colorBoxWidth: {
      type: Number,
      required: true
    },
    colorBoxHeight: {
      type: Number,
      required: true
    },
    defaultColor: {
      type: Object as PropType<HSB>,
      required: true
    },
    colorName: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      isMouseDown: false,
      colorBoxProps: {} as DOMRect | undefined,
      selectedColor: {} as ColorGroup
    };
  },
  methods: {
    newMousePos(e: MouseEvent) {
      if (e.type === "mousedown") {
        e.preventDefault();
        this.isMouseDown = true;
        this.colorBoxProps = document
          .querySelector(`#${this.colorName}-color-box`)
          ?.getBoundingClientRect();
        this.setNewMarkerPos(e);
        return;
      }
      if (e.type === "mouseup") {
        this.isMouseDown = false;
        return;
      }
      if (e.type === "mousemove" && this.isMouseDown === true) {
        this.setNewMarkerPos(e);
        return;
      }
    },
    setNewMarkerPos(e: MouseEvent) {
      if (this.colorBoxProps) {
        let markerLeft =
          ((e.x - this.colorBoxProps.left) / this.colorBoxProps.width) * 100;
        let markerTop =
          ((e.y - this.colorBoxProps.top) / this.colorBoxProps.height) * 100;

        markerLeft = markerLeft > 100 ? 100 : markerLeft;
        markerTop = markerTop > 100 ? 100 : markerTop;
        markerLeft = markerLeft < 0 ? 0 : markerLeft;
        markerTop = markerTop < 0 ? 0 : markerTop;

        this.selectedColor.hsb.s = Math.round(markerLeft);
        this.selectedColor.hsb.b = Math.round(100 - markerTop);
      }
    },
    HSBtoHSL: function(hsb: HSB): HSL {
      // Takes hsb where h[0, 360] s[0, 100] b[0, 100]
      // Returns hsl where h[0, 360] l[0, 100] l[0, 100]
      const s = hsb.s / 100;
      const b = hsb.b / 100;
      let saturation;
      let lightness = b * (1 - s / 2);

      if (lightness === 0 || lightness === 1) {
        saturation = 0;
      } else {
        saturation = (b - lightness) / Math.min(lightness, 1 - lightness);
      }

      saturation = saturation * 100;
      lightness = lightness * 100;

      saturation = Math.round(saturation) <= 100 ? Math.round(saturation) : 100;
      lightness = Math.round(lightness) <= 100 ? Math.round(lightness) : 100;

      const res = {
        h: hsb.h,
        s: saturation,
        l: lightness
      };

      return res;
    },
    HSBtoRGB: function(hsb: HSB): RGB {
      const h = hsb.h / 360;
      const s = hsb.s / 100;
      const b = hsb.b / 100;

      let red = 1;
      let green = 1;
      let blue = 1;
      const i = Math.floor(h * 6);
      const f = h * 6 - i;
      const p = b * (1 - s);
      const q = b * (1 - f * s);
      const t = b * (1 - (1 - f) * s);
      switch (i % 6) {
        case 0:
          (red = b), (green = t), (blue = p);
          break;
        case 1:
          (red = q), (green = b), (blue = p);
          break;
        case 2:
          (red = p), (green = b), (blue = t);
          break;
        case 3:
          (red = p), (green = q), (blue = b);
          break;
        case 4:
          (red = t), (green = p), (blue = b);
          break;
        case 5:
          (red = b), (green = p), (blue = q);
          break;
      }
      const res = {
        r: Math.round(red * 255),
        g: Math.round(green * 255),
        b: Math.round(blue * 255)
      };

      return res;
    },
    RGBtoHEX: function(rgb: RGB): HEX {
      function componentToHex(c: number): string {
        const hex = c.toString(16);
        return hex.length == 1 ? "0" + hex : hex;
      }
      const res = {
        r: componentToHex(rgb.r),
        g: componentToHex(rgb.g),
        b: componentToHex(rgb.b)
      };

      return res;
    }
  },
  created() {
    this.selectedColor = {
      hsb: this.defaultColor,
      hsl: {} as HSL,
      rgb: {} as RGB,
      hex: {} as HEX
    };
  },
  mounted() {
    window.addEventListener("mousemove", this.newMousePos);
    window.addEventListener("mouseup", this.newMousePos);
  },
  beforeUnmount() {
    window.removeEventListener("mousemove", this.newMousePos);
    window.removeEventListener("mouseup", this.newMousePos);
  },
  computed: {
    selectedColorHSB(): HSB {
      return this.selectedColor.hsb;
    },
    selectedColorHSL(): string {
      return `hsl(${this.selectedColor.hsl.h}, ${this.selectedColor.hsl.s}%, ${this.selectedColor.hsl.l}%)`;
    },
    selectedColorRGB(): string {
      return `rgb(${this.selectedColor.rgb.r}, ${this.selectedColor.rgb.g}, ${this.selectedColor.rgb.b})`;
    },
    selectedColorHEX(): string {
      return `#${this.selectedColor.hex.r}${this.selectedColor.hex.g}${this.selectedColor.hex.b}`;
    }
  },
  watch: {
    hsbSaturation(newVal) {
      this.selectedColor.hsb.s = newVal;
    },
    hsbBrightness(newVal) {
      this.selectedColor.hsb.b = newVal;
    },
    selectedColorHSB: {
      deep: true,
      handler() {
        this.selectedColor.hsl = this.HSBtoHSL(this.selectedColor.hsb);
        this.selectedColor.rgb = this.HSBtoRGB(this.selectedColor.hsb);
        this.selectedColor.hex = this.RGBtoHEX(this.selectedColor.rgb);
      }
    },
    selectedColor: {
      deep: true,
      handler() {
        this.$emit("colorChange", this.selectedColor);
      }
    }
  },
  emits: ["colorChange"]
});
</script>

<style scoped>
.container {
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 0.5rem;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  font-size: 0.875rem;
  background-color: white;
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
  border: 1px solid #fafafa;
  border-radius: 0.25rem;
  background-color: #fafafa;
}
.color-prop input:focus {
  outline: none;
  border: 1px solid #a3a3a3;
}
.color-prop input::-webkit-inner-spin-button {
  appearance: none;
}
.color-string {
  margin-top: 0.75rem;
}
</style>
