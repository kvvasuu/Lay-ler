<template>
  <div
    @click="() => this.$emit('toggle-modal')"
    class="pallet-modal"
    @keydown.enter="updatePallet"
  >
    <div class="pallet-modal-content" @click.stop="">
      <div class="close" @click="() => this.$emit('toggle-modal')"></div>
      <div class="pallet-number">{{ pallet.number + 1 }}</div>
      <div class="pallet-name">
        <input
          type="text"
          class="name-input"
          v-model="palletName"
          placeholder="Name"
        />
      </div>
      <div class="pallet-modal-inner">
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
          /><label class="pallet-input-label" for="length">Length:</label>
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
          /><label class="pallet-input-label" for="width">Width:</label>
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
      </div>
      <color-picker
        :color="this.palletColor"
        @update-color="(color) => (palletColor = color)"
      ></color-picker>
      <div class="form-button">
        <button class="button-set" @click="updatePallet">Set</button>
        <button class="button-set" @click="updateAllPallets">Set all</button>
        <button class="button-delete" @click="deletePallet">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
import ColorPicker from "./ColorPicker.vue";

export default {
  components: {
    ColorPicker,
  },
  props: ["pallet"],
  emits: [
    "toggle-modal",
    "update-pallet",
    "update-all-pallets",
    "delete-pallet",
  ],
  data() {
    return {
      palletLength: this.pallet.length,
      palletWidth: this.pallet.width,
      palletName: this.pallet.name,
      palletColor: this.pallet.color,
      interval: null,
      isMouseDown: false,
    };
  },
  methods: {
    updatePallet() {
      this.pallet.length = this.palletLength;
      this.pallet.width = this.palletWidth;
      this.pallet.name = this.palletName;
      this.pallet.color = this.palletColor;
      this.$emit("update-pallet");
      this.$emit("toggle-modal");
    },
    updateAllPallets() {
      this.updatePallet();
      this.$emit("update-all-pallets", this.pallet);
    },
    increaseInput(ref) {
      let step = 0.1;
      this[ref.name] = Math.round(Number(this[ref.name] + step) * 100) / 100;
    },
    decreaseInput(ref) {
      let step = 0.1;
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
    mouseUp() {
      this.isMouseDown = false;
      clearInterval(this.interval);
    },
    deletePallet() {
      this.$emit("delete-pallet", this.pallet.number);
    },
  },
  watch: {
    palletLength() {
      if (this.palletLength > 13.6) {
        this.palletLength = 13.6;
      }
      if (this.palletLength < 0.4) {
        this.palletLength = 0.4;
      }
    },
    palletWidth() {
      if (this.palletWidth > 2.4) {
        this.palletWidth = 2.4;
      }
      if (this.palletWidth < 0.4) {
        this.palletWidth = 0.4;
      }
    },
  },
};
</script>

<style scoped>
.pallet-modal {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 99;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.185);
  backdrop-filter: blur(2px);
  cursor: default;
}

.pallet-modal-content {
  position: inherit;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: auto;
  width: 10rem;
  margin: 0.2rem;
  border-radius: 0.6rem;
  border: 2px solid #c2c2c270;
  background-color: rgba(54, 54, 56, 1);
}

.pallet-modal-inner {
  display: flex;
  align-items: center;
  justify-content: center;
}

.pallet-number {
  position: absolute;
  top: 2%;
  left: 5%;
  font-size: 0.9rem;
  font-weight: bold;
  text-transform: uppercase;
  margin: 0.2rem;
}

.pallet-name {
  font-size: 0.3rem;
  font-weight: bold;
  text-transform: uppercase;
  margin: 0.8rem 0 0 0;
}

.name-input {
  outline: none !important;
  background-color: transparent;
  padding: 0.14rem 0.1rem 0.08rem 0.1rem;
  border: none;
  border-bottom: 1.5px solid #bdbdbd;
  text-align: center;
  color: #bdbdbd;
  height: 0.6rem;
  font-size: 0.35rem;
  font-weight: bold;
  transition: all 0.3s ease;
  padding: 0;
  margin: 0.6rem;
}

.name-input:focus {
  border-color: rgb(241, 165, 94);
  color: rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.form-button {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  margin: 0 0 0.6rem 0;
}

.pallet-input-group {
  position: relative;
  margin: 0.2rem;
}

.pallet-input {
  outline: none !important;
  background-color: #343435;
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
  background-color: #343435;
  cursor: default;
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.pallet-input:focus ~ .pallet-input-label {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
}

.button-set {
  background-color: #343435;
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
  margin: 0 0.2rem 0.2rem 0.2rem;
}

.button-delete {
  color: #cc3333;
  background-color: transparent;
  cursor: pointer;
  border: 1px solid #cc3333;
  border-radius: 2rem;
  padding: 0.1rem;
  text-align: center;
  width: 1.4rem;
  font-size: 0.3rem;
  font-weight: bold;
  transition: color 0.3s ease, border 0.3s ease, filter 0.3s ease;
  margin: 0 0.2rem 0.2rem 0.2rem;
}

.button-delete:hover {
  color: #cc3333;
  border: 1px solid #cc3333;
  filter: drop-shadow(0 0 1px #cc3333);
}

.button-set:hover {
  color: rgb(241, 165, 94);
  border: 1px solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.button-set:active {
  transform: translate(0, 1px);
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
  border-top: 4px solid #bdbdbd;
  border-left: 4px solid #bdbdbd;
  rotate: 45deg;
}

.arrow-down {
  position: absolute;
  width: 0.18rem;
  height: 0.18rem;
  bottom: 60%;
  border: none;
  border-top: 4px solid #bdbdbd;
  border-left: 4px solid #bdbdbd;
  rotate: -135deg;
}

.arrow:hover {
  border-top: 4px solid rgb(241, 165, 94);
  border-left: 4px solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.arrow:active {
  transform: scale(0.9);
}

@media screen and (max-width: 640px) {
  .pallet-input-arrow-up,
  .pallet-input-arrow-down {
    display: none;
  }
  .pallet-input {
    width: 1.4rem;
  }
}

@media screen and (max-width: 1000px) {
  .pallet-modal-content {
    width: auto;
  }
  .name-input {
    margin: 0.6rem 0.2rem 0.6rem 0.2rem;
    max-width: 3rem;
  }
}

input[type="text"] {
  appearance: none;
  -webkit-appearance: none;
  border-radius: 0;
  -webkit-border-radius: 0px;
}
</style>
