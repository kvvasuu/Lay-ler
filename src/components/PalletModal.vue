<template>
  <div @click="() => this.$emit('toggle-modal')" class="pallet-modal">
    <div class="pallet-modal-content" @click.stop="">
      <div class="pallet-number">{{ pallet.number + 1 }}</div>
      <div class="pallet-name">
        <input
          type="text"
          class="name-input"
          v-model="palletName"
          ref="palletNameInput"
        />
      </div>
      <div class="pallet-modal-inner">
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
      </div>
      <div class="form-button">
        <button class="button-set" @click="updatePallet">Set</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["pallet"],
  emits: ["toggle-modal", "update-pallet"],
  data() {
    return {
      palletLength: this.pallet.length,
      palletWidth: this.pallet.width,
      palletName: this.pallet.name,
    };
  },
  methods: {
    updatePallet() {
      this.pallet.length = this.palletLength;
      this.pallet.width = this.palletWidth;
      this.pallet.name = this.palletName;
      this.$emit("update-pallet");
      this.$emit("toggle-modal");
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
  mounted() {
    this.$refs.palletNameInput.focus();
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
  cursor: default;
}

.pallet-modal-content {
  position: inherit;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 10rem;
  height: 10rem;
  border-radius: 0.6rem;
  background-color: rgba(54, 54, 56, 0.5);
  backdrop-filter: blur(30px);
}

.pallet-modal-inner {
  display: flex;
  align-items: center;
  justify-content: center;
}

.pallet-number {
  position: absolute;
  top: 5%;
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
  animation-name: input-blink;
  animation-duration: 1.5s;
  animation-iteration-count: infinite;
  margin: 0.6rem;
}

.name-input:focus {
  border-color: rgb(241, 165, 94);
  animation: none;
}

@keyframes input-blink {
  0% {
    border-color: #bdbdbd00;
  }
  50% {
    border-color: #bdbdbd;
  }
  100% {
    border-color: #bdbdbd00;
  }
}

.form-button {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0.2rem;
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
  background-color: #2a2933;
  cursor: default;
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.pallet-input:focus ~ .pallet-input-label {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
}

.button-set {
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

.button-set:hover {
  color: rgb(241, 165, 94);
  border: 1.5px solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
}

.button-set:active {
  transform: translate(0, 1px);
}
</style>
