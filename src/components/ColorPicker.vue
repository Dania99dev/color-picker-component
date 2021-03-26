<template>
  <div class="color-picker-container">
    <span
      :id="`${colorName}-color-sample`"
      @click="colorSelectorVisibility = !colorSelectorVisibility"
      :style="{ backgroundColor: selectedColorHEXstring }"
    ></span>
    <span :id="`${colorName}-color-code`">{{
      selectedColorHEXstring.toUpperCase()
    }}</span>
    <div class="color-selector-container" v-if="colorSelectorVisibility">
      <ColorSelector
        :colorBoxWidth="250"
        :colorBoxHeight="200"
        :defaultColor="defaultColor"
        @colorChange="newColor => (selectedColor = newColor)"
        :colorName="colorName"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ColorSelector from "@/components/ColorSelector.vue";
import { ColorGroup, HSB, HSL, RGB, HEX } from "@/types";

export default defineComponent({
  name: "ColorPicker",
  components: {
    ColorSelector
  },
  props: {
    colorName: {
      type: String,
      reqired: true
    }
  },
  data() {
    return {
      colorSelectorVisibility: false,
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
  beforeMount() {
    this.colorSelectorVisibility = true;
    this.$nextTick(() => {
      this.colorSelectorVisibility = false;
    });
  },
  computed: {
    selectedColorHEXstring(): string {
      return `#${this.selectedColor.hex.r}${this.selectedColor.hex.g}${this.selectedColor.hex.b}`;
    }
  }
});
</script>

<style scoped>
.color-picker-container {
  border: 1px solid #d4d4d4;
  display: flex;
  align-items: center;
  padding: 0.5rem;
  border-radius: 0.5rem;
  margin: 1rem;
  position: relative;
}
.color-selector-container {
  position: absolute;
  left: 105%;
  top: 10%;
}
span[id$="-color-sample"] {
  display: inline-block;
  width: 40px;
  height: 40px;
  border-radius: 0.5rem;
}
span[id$="-color-code"] {
  background-color: #fafafa;
  height: 40px;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  margin-left: 0.5rem;
  border-radius: 0.5rem;
  width: 8rem;
}
</style>
