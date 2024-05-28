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
    <hr />
  </div>
  <div class="trailer-container">
    <trailer
      :trailerLength="trailerLength"
      :trailerWidth="trailerWidth"
      :pallets="passPallets"
    ></trailer>
    <input
      class="trailer-length trailer-size"
      v-model="trailerLength"
      type="number"
      min="2"
      max="20"
    />
    <input
      class="trailer-width trailer-size"
      v-model="trailerWidth"
      type="number"
      min="1"
      max="4"
    />
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
      trailerLength: 13.6,
      trailerWidth: 2.45,
      palletLength: 1.2,
      palletWidth: 0.8,
      numberOfPallets: 0,
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
      let maxRows = Math.floor(this.trailerWidth / this.palletWidth);
      let maxCols = Math.floor(this.trailerLength / this.palletLength);
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
    trailerLength() {
      this.calculateNumberOfPallets();
      this.arrangePallets();
      if (this.trailerLength > 15) {
        this.trailerLength = 15;
      }
    },
    trailerWidth() {
      this.calculateNumberOfPallets();
      this.arrangePallets();
      if (this.trailerWidth > 4) {
        this.trailerWidth = 4;
      }
    },
  },
  computed: {
    passPallets() {
      return this.pallets;
    },
  },
};
</script>

<style scoped>
.trailer-container {
  position: relative;
  margin: 1rem;
}

.trailer-length {
  position: absolute;
  bottom: -32px;
  left: 50%;
  transform: translate(-50%, 0);
  border: none;
  width: 50px;
}

.trailer-width {
  position: absolute;
  top: 50%;
  right: -58px;
  transform: translate(0, -50%);
  width: 40px;
  border: none;
}

.trailer-size {
  background-color: transparent;
  text-align: center;
  font-size: 20px;
}

.trailer-size:focus {
  outline: none !important;
  border-bottom: 1px solid transparent;
}

.trailer-size::-webkit-outer-spin-button,
.trailer-size::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
