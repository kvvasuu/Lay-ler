<template>
  <div class="title">Layler</div>
  <div class="description">
    Your tool for optimal pallet placement on truck trailers. Enter the
    dimensions and quantity of pallets, and our app will automatically plan the
    most efficient layout, saving you time and space.<br />
    The perfect solution for logistics and transportation!
  </div>
  <div class="form-container">
    <div class="form-inputs">
      <div class="pallet-input-group">
        <input
          v-model="palletLength"
          name="palletLength"
          ref="palletLength"
          type="number"
          step="0.1"
          min="0.4"
          max="13.6"
          class="pallet-input"
        /><label class="pallet-input-label" for="palletLength">Length:</label>
        <div
          class="pallet-input-arrow-up"
          @mousedown="mouseDown(this.$refs.palletLength, 'increase')"
          @mouseout="mouseUp"
          @mouseup="mouseUp"
        >
          <div class="arrow-up arrow"></div>
        </div>
        <div
          class="pallet-input-arrow-down"
          @mousedown="mouseDown(this.$refs.palletLength, 'decrease')"
          @mouseout="mouseUp"
          @mouseup="mouseUp"
        >
          <div class="arrow-down arrow"></div>
        </div>
      </div>
      <div class="pallet-input-group">
        <input
          v-model="palletWidth"
          name="palletWidth"
          ref="palletWidth"
          type="number"
          step="0.1"
          min="0.4"
          max="2.4"
          class="pallet-input"
        /><label class="pallet-input-label" for="palletWidth">Width:</label>
        <div
          class="pallet-input-arrow-up"
          @mousedown="mouseDown(this.$refs.palletWidth, 'increase')"
          @mouseout="mouseUp"
          @mouseup="mouseUp"
        >
          <div class="arrow-up arrow"></div>
        </div>
        <div
          class="pallet-input-arrow-down"
          @mousedown="mouseDown(this.$refs.palletWidth, 'decrease')"
          @mouseout="mouseUp"
          @mouseup="mouseUp"
        >
          <div class="arrow-down arrow"></div>
        </div>
      </div>
      <div class="pallet-input-group">
        <input
          v-model="numberOfPallets"
          name="numberOfPallets"
          ref="numberOfPallets"
          type="number"
          step="1"
          min="0"
          max="204"
          class="pallet-input"
        /><label class="pallet-input-label" for="numberOfPallets"
          >Number:</label
        >
        <div
          class="pallet-input-arrow-up"
          @mousedown="mouseDown(this.$refs.numberOfPallets, 'increase')"
          @mouseout="mouseUp"
          @mouseup="mouseUp"
        >
          <div class="arrow-up arrow"></div>
        </div>
        <div
          class="pallet-input-arrow-down"
          @mousedown="mouseDown(this.$refs.numberOfPallets, 'decrease')"
          @mouseout="mouseUp"
          @mouseup="mouseUp"
        >
          <div class="arrow-down arrow"></div>
        </div>
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
      min="1"
      max="20"
      step="0.01"
    />
    <input
      class="trailer-width trailer-size"
      v-model="trailerWidth"
      type="number"
      min="1"
      max="4"
      step="0.01"
    />
  </div>
  <div class="state-buttons">
    <button class="button-sort state-button" @click="resetState">Reset</button>
    <button class="button-sort state-button" @click="saveState">Save</button>
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
      numberOfPallets: 7,
      pallets: [],
      step: 0.1,
      sort: false,
      interval: null,
      isMouseDown: false,
    };
  },
  methods: {
    arrangePallets() {
      this.pallets = [];
      let temp = this.numberOfPallets;
      let number = 0;
      while (temp > 0) {
        this.pallets.push({
          length: this.palletLength,
          width: this.palletWidth,
          number: number,
          name: "",
          color: "#dfa36c",
        });
        temp--;
        number++;
      }
    },
    calculateNumberOfPallets() {
      let maxRows = Math.floor(this.trailerWidth / this.palletWidth);
      let maxCols = Math.floor(this.trailerLength / this.palletLength);
      if (maxCols * maxRows < this.numberOfPallets) {
        this.numberOfPallets = maxCols * maxRows;
      }
    },
    increaseInput(ref) {
      let step = 0.1;
      if (ref.name == "numberOfPallets") step = 1;
      this[ref.name] = Math.round(Number(this[ref.name] + step) * 100) / 100;
    },
    decreaseInput(ref) {
      let step = 0.1;
      if (ref.name == "numberOfPallets") step = 1;
      if (this[ref.name] > 0)
        this[ref.name] = Math.round(Number(this[ref.name] - step) * 100) / 100;
    },
    async mouseDown(ref, type) {
      this.isMouseDown = true;
      switch (type) {
        case "increase":
          this.increaseInput(ref);
          this.interval = setInterval(() => {
            this.increaseInput(ref);
          }, 300);
          setTimeout(() => {
            clearInterval(this.interval);
            if (this.isMouseDown) {
              this.interval = setInterval(() => {
                this.increaseInput(ref);
              }, 100);
            }
          }, 1000);
          break;
        case "decrease":
          this.decreaseInput(ref);
          this.interval = setInterval(() => {
            this.decreaseInput(ref);
          }, 300);
          setTimeout(() => {
            clearInterval(this.interval);
            if (this.isMouseDown) {
              this.interval = setInterval(() => {
                this.decreaseInput(ref);
              }, 100);
            }
          }, 1000);
          break;
      }
    },
    saveState() {
      localStorage.setItem("pallets", JSON.stringify(this.pallets));
      localStorage.setItem("trailerLength", JSON.stringify(this.trailerLength));
      localStorage.setItem("trailerWidth", JSON.stringify(this.trailerWidth));
      localStorage.setItem("palletLength", JSON.stringify(this.palletLength));
      localStorage.setItem("palletWidth", JSON.stringify(this.palletWidth));
      localStorage.setItem(
        "numberOfPallets",
        JSON.stringify(this.numberOfPallets)
      );
      localStorage.setItem("sort", JSON.stringify(this.sort));
    },
    loadState() {
      if (localStorage.getItem("pallets")) {
        this.pallets = JSON.parse(localStorage.getItem("pallets"));
        this.trailerLength = JSON.parse(localStorage.getItem("trailerLength"));
        this.trailerWidth = JSON.parse(localStorage.getItem("trailerWidth"));
        this.palletLength = JSON.parse(localStorage.getItem("palletLength"));
        this.palletWidth = JSON.parse(localStorage.getItem("palletWidth"));
        this.numberOfPallets = JSON.parse(
          localStorage.getItem("numberOfPallets")
        );
        this.sort = JSON.parse(localStorage.getItem("sort"));
        console.log(this.pallets[0]);
      } else {
        this.resetState();
      }
    },
    resetState() {
      (this.trailerLength = 13.6),
        (this.trailerWidth = 2.45),
        (this.palletLength = 1.2),
        (this.palletWidth = 0.8),
        (this.numberOfPallets = 7),
        (this.sort = false);
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    mouseUp() {
      this.isMouseDown = false;
      clearInterval(this.interval);
    },
  },
  watch: {
    numberOfPallets() {
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    palletLength() {
      if (this.palletLength > this.trailerLength) {
        this.palletLength = this.trailerLength;
      }
      if (this.palletLength < 0.4) {
        this.palletLength = 0.4;
      }
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    palletWidth() {
      if (this.palletWidth > this.trailerWidth) {
        this.palletWidth = this.trailerWidth;
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
      if (this.trailerLength < this.palletLength) {
        this.palletLength = this.trailerLength;
      }
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
    trailerWidth() {
      if (this.trailerWidth > 4) {
        this.trailerWidth = 4;
      }
      if (this.trailerWidth < this.palletWidth) {
        this.palletWidth = this.trailerWidth;
      }
      this.calculateNumberOfPallets();
      this.arrangePallets();
    },
  },
  computed: {
    passPallets() {
      console.log(this.pallets);
      return this.pallets;
    },
  },
  mounted() {
    this.loadState();
  },
};
</script>

<style scoped>
.title {
  font-size: 0.7rem;
  font-weight: bold;
  margin: 0 0 0.1rem 0;
}

.description {
  width: 10rem;
  text-align: center;
  margin: 0 0 0.6rem 0;
}

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
  padding: 0.14rem 0.1rem 0.08rem 0.1rem;
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
  top: -20%;
  left: 15%;
  background-color: transparent;
  cursor: default;
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.pallet-input:focus ~ .pallet-input-label {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
}

.pallet-input-arrow-up {
  position: absolute;
  width: 0.4rem;
  height: 0.34rem;
  top: 0;
  right: 0.1rem;
  background-color: transparent;
  border: none;
  cursor: pointer;
}
.pallet-input-arrow-down {
  position: absolute;
  width: 0.4rem;
  height: 0.34rem;
  bottom: 0;
  right: 0.1rem;
  background-color: transparent;
  cursor: pointer;
}

.arrow-up {
  position: absolute;
  width: 0.18rem;
  height: 0.18rem;
  top: 60%;
  border: none;
  border-top: 0.06rem solid #bdbdbd;
  border-left: 0.06rem solid #bdbdbd;
  rotate: 45deg;
}

.arrow-down {
  position: absolute;
  width: 0.18rem;
  height: 0.18rem;
  bottom: 60%;
  border: none;
  border-top: 0.06rem solid #bdbdbd;
  border-left: 0.06rem solid #bdbdbd;
  rotate: -135deg;
}

.arrow:hover {
  border-top: 0.06rem solid rgb(241, 165, 94);
  border-left: 0.06rem solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.arrow:active {
  transform: scale(0.9);
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

.state-buttons {
  margin-top: 0.6rem;
  margin-left: auto;
  margin-right: 0.6rem;
}

.state-button {
  margin: 0 0 0 0.2rem;
}

@media screen and (max-width: 600px) {
  .form-inputs {
    flex-direction: column;
  }
}

/* @media screen and (max-width: 800px) {
  .trailer-container {
    rotate: 90deg;
  }
} */
</style>
