<template>
  <div class="form-container">
    <label for="length">Length:</label
    ><input
      v-model="palletLength"
      name="length"
      type="number"
      step="0.1"
      min="0.4"
      max="13.6"
    />
    <label for="width">Width:</label
    ><input
      v-model="palletWidth"
      name="width"
      type="number"
      step="0.1"
      min="0.4"
      max="2.4"
    />
    <label for="numberOfPallets">Number of pallets:</label
    ><input
      v-model="numberOfPallets"
      name="numberOfPallets"
      type="number"
      step="1"
      min="0"
      max="204"
    />
    <button @click="arrangePallets">Arrange</button>
  </div>
  <div class="trailer-container">
    <trailer :size="trailerSize" :pallets="passPallets"></trailer>
  </div>
</template>

<script>
import Trailer from "./components/Trailer.vue";

export default {
  components: {
    Trailer,
  },
  data() {
    return {
      trailerSize: { length: 13.6, width: 2.45 },
      palletLength: 1.2,
      palletWidth: 0.8,
      numberOfPallets: 1,
      pallets: [],
    };
  },
  methods: {
    arrangePallets() {
      this.pallets = [];
      let temp = this.numberOfPallets;
      while (temp > 0) {
        this.pallets.push({
          length: this.palletLength,
          width: this.palletWidth,
        });
        temp--;
      }
    },
    calculateNumberOfPallets() {
      let maxRows = Math.floor(this.trailerSize.width / this.palletWidth);
      let maxCols = Math.floor(this.trailerSize.length / this.palletLength);
      console.log(maxCols);
      console.log(this.palletLength * 0.02);
      if (maxCols * maxRows < this.numberOfPallets) {
        this.numberOfPallets = maxCols * maxRows;
      }
    },
  },
  watch: {
    numberOfPallets() {
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    palletLength() {
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    palletWidth() {
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
  },
  computed: {
    passPallets() {
      return this.pallets;
    },
  },
};
</script>

<style scoped></style>
