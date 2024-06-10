<template>
  <div class="color-section">
    <div class="color-name">Color:</div>
    <div class="color-preview" :style="{ 'background-color': color }"></div>
    <div class="colors">
      <div
        class="color"
        v-for="(color, index) in colors"
        :key="index"
        :style="{ 'background-color': color }"
        @click="updateColor(color)"
        :name="color"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["color"],
  emits: ["update-color"],
  data() {
    return {
      palletColor: this.color,
      colors: [
        "#DFA36C",
        "#C0392B",
        "#E74C3C",
        "#9B59B6",
        "#8E44AD",
        "#2980B9",
        "#3498DB",
        "#1ABC9C",
        "#16A085",
        "#27AE60",
        "#2ECC71",
        "#F1C40F",
        "#F39C12",
        "#E67E22",
        "#D35400",
        "#BDC3C7",
        "#95A5A6",
        "#7F8C8D",
        "#34495E",
        "#251f19",
      ],
    };
  },
  methods: {
    updateColor(color) {
      this.palletColor = color;
      this.$emit("update-color", this.palletColor);
    },
  },
};
</script>

<style scoped>
.color-section {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 0;
}

.color-name {
  margin: 0.1rem;
}

.color-preview {
  border-radius: 0.2rem;
  border: 2px solid #bdbdbd;
  width: 1rem;
  height: 1rem;
  transition: all 0.3s ease;
}

.colors {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  max-width: 6.4rem;
  margin: 0.4rem;
}

.color {
  border-radius: 0.14rem;
  border: 2px solid #bdbdbd;
  width: 0.5rem;
  height: 0.5rem;
  margin: 0.02rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.color:hover {
  border-color: rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
  width: 0.54rem;
  height: 0.54rem;
}

.close {
  position: absolute;
  right: 0.5rem;
  top: 0.5rem;
  width: 0.5rem;
  height: 0.5rem;
  z-index: 10;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close:hover {
  filter: drop-shadow(0 0 6px rgb(160, 113, 70));
}

.close:hover:before,
.close:hover:after {
  background-color: rgb(241, 165, 94);
}

.close:before,
.close:after {
  position: absolute;
  left: 0.2rem;
  content: " ";
  height: 0.6rem;
  width: 0.06rem;
  background-color: #bdbdbd;
}
.close:before {
  transform: rotate(45deg);
}
.close:after {
  transform: rotate(-45deg);
}
</style>
