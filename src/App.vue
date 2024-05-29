<template>
  <div class="form-container">
    <div class="form-inputs">
      <div class="pallet-input-group">
        <input
          v-model="palletLength"
          name="length"
          type="number"
          step="0.1"
          min="0.4"
          max="13.6"
          class="pallet-input"
        /><label class="pallet-input-label" for="length">Length:</label>
      </div>
      <div class="pallet-input-group">
        <input
          v-model="palletWidth"
          name="length"
          type="number"
          step="0.1"
          min="0.4"
          max="2.4"
          class="pallet-input"
        /><label class="pallet-input-label" for="width">Width:</label>
      </div>
      <div class="pallet-input-group">
        <input
          v-model="numberOfPallets"
          name="numberOfPallets"
          type="number"
          step="1"
          min="0"
          max="204"
          class="pallet-input"
        /><label class="pallet-input-label" for="numberOfPallets"
          >Number:</label
        >
      </div>
    </div>
    <div class="form-button">
      <button
        class="button-sort"
        :class="{ 'button-sort-active': sort }"
        @click="sort = !sort"
      >
        Sort
      </button>
    </div>
  </div>
  <div class="trailer-container">
    <trailer
      :trailerLength="trailerLength"
      :trailerWidth="trailerWidth"
      :pallets="passPallets"
      :sort="sort"
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
      sort: false,
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
      if (this.palletLength > 13.6) {
        this.palletLength = 13.6;
      }
      if (this.palletLength < 0.4) {
        this.palletLength = 0.4;
      }
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    palletWidth() {
      if (this.palletWidth > 2.4) {
        this.palletWidth = 2.4;
      }
      if (this.palletWidth < 0.4) {
        this.palletWidth = 0.4;
      }
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    trailerLength() {
      if (this.trailerLength > 15) {
        this.trailerLength = 15;
      }
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    trailerWidth() {
      if (this.trailerWidth > 4) {
        this.trailerWidth = 4;
      }
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

<style scoped>
.form-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.form-inputs {
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-button {
  display: flex;
  align-items: center;
  justify-content: center;
}

.pallet-input-group {
  position: relative;
  margin: 0.5rem;
}

.pallet-input {
  outline: none !important;
  background-color: transparent;
  border: 1px solid #bdbdbd;
  border-radius: 2rem;
  padding: 0.1rem;
  text-align: center;
  color: #bdbdbd;
  width: 2rem;
  height: 0.6rem;
  font-size: 0.35rem;
  font-weight: bold;
  transition: all 0.3s ease;
}

.pallet-input:focus {
  color: rgb(223, 163, 108);
  outline: none !important;
  border: 1.5px solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.pallet-input-label {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #bdbdbd;
  font-weight: bold;
  padding: 0 5px;
  border: 1.5px solid #bdbdbd;
  border-radius: 20px;
  top: -11.5px;
  left: 25px;
  background-color: transparent;
  cursor: default;
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.pallet-input:focus ~ .pallet-input-label {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
}

.button-sort {
  background-color: transparent;
  cursor: pointer;
  border: 1px solid #bdbdbd;
  border-radius: 2rem;
  padding: 0.1rem;
  text-align: center;
  color: #bdbdbd;
  width: 1.4rem;
  font-size: 0.3rem;
  font-weight: bold;
  transition: color 0.3s ease, border 0.3s ease, filter 0.3s ease;
}

.button-sort:hover {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.button-sort:active {
  transform: translate(0, 1px);
}

.button-sort-active {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
}

.trailer-container {
  position: relative;
  margin: 0.5rem;
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
  right: -50px;
  transform: translate(0, -50%);
  width: 40px;
  border: none;
}

.trailer-size {
  background-color: transparent;
  text-align: center;
  font-size: 20px;
  color: #bdbdbd;
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

hr {
  border: 0;
  height: 1px;
  width: 70%;
  margin: 0;
  background-image: linear-gradient(
    to right,
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 0.75),
    rgba(0, 0, 0, 0)
  );
}
</style>
