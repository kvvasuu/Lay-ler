<template>
  <div
    class="trailer"
    :style="{ height: passHeight + 'rem', width: passWidth + 'rem' }"
    ref="trailer"
  >
    <TransitionGroup name="fade">
      <pallet
        v-for="pallet in pallets"
        :key="pallet.number"
        class="pallet"
        :pallet="pallet"
        @sort="sortPallets"
        @update-pallet="sortPallets"
        ref="pallets"
        :style="{ 'background-color': pallet.color }"
        @update-all-pallets="(pallet) => updateAllPallets(pallet)"
        :rotate="rotate"
        @replace-pallets="
          (draggedPallet, palletToReplace) =>
            replacePallets(draggedPallet, palletToReplace)
        "
        @delete-pallet="(number) => deletePallet(number)"
      ></pallet>
    </TransitionGroup>
    <Transition name="fade"
      ><div v-if="unloading" class="unloading">Unloading...</div></Transition
    >
  </div>
</template>

<script>
import Pallet from "./Pallet.vue";

export default {
  components: {
    Pallet,
  },
  props: ["trailerLength", "trailerWidth", "pallets", "sort", "rotate"],
  emits: ["update-all-pallets", "unloading"],
  data() {
    return {
      lastPalletPosition: 0,
      trailerRightBorderPosition: 0,
      unloading: false,
    };
  },
  methods: {
    async sortPallets() {
      setTimeout(() => {
        if (this.sort)
          this.pallets.sort((a, b) => b.length * b.width - a.length * a.width);
        this.calculateNumberOfPallets();
      }, 500);
    },
    async calculateNumberOfPallets() {
      !this.rotate
        ? (this.trailerRightBorderPosition =
            this.$refs.trailer.getBoundingClientRect().right)
        : (this.trailerRightBorderPosition =
            this.$refs.trailer.getBoundingClientRect().bottom);

      setTimeout(() => {
        !this.rotate
          ? (this.lastPalletPosition =
              this.$refs.pallets[
                this.$refs.pallets.length - 1
              ].$el.getBoundingClientRect().right)
          : (this.lastPalletPosition =
              this.$refs.pallets[
                this.$refs.pallets.length - 1
              ].$el.getBoundingClientRect().bottom);

        if (this.lastPalletPosition > this.trailerRightBorderPosition) {
          this.unloading = true;
          this.pallets.pop();
          this.calculateNumberOfPallets();
          this.$emit("unloading", this.unloading, this.pallets.length);
        } else {
          this.unloading = false;
          this.$emit("unloading", this.unloading, this.pallets.length);
        }
      }, 200);
    },
    updateAllPallets(pallet) {
      this.pallets.map((el) => {
        el.length = pallet.length;
        el.width = pallet.width;
        el.name = pallet.name;
        el.color = pallet.color;
      });
      this.$emit("update-all-pallets", pallet);
    },
    replacePallets(draggedPallet, palletToReplace) {
      const draggedPalletIndex = this.pallets
        .map((el) => el.number)
        .indexOf(Number(draggedPallet));
      const draggedPalletTemp = this.pallets[draggedPalletIndex];
      const palletToReplaceIndex = this.pallets
        .map((el) => el.number)
        .indexOf(Number(palletToReplace));

      this.pallets.splice(draggedPalletIndex, 1);
      this.pallets.splice(palletToReplaceIndex, 0, draggedPalletTemp);
    },
    deletePallet(number) {
      const palletToDelete = this.pallets
        .map((el) => el.number)
        .indexOf(Number(number));
      setTimeout(() => {
        this.pallets.splice(palletToDelete, 1);
      }, 200);
    },
  },
  watch: {
    sort() {
      this.sortPallets();
    },
    pallets: {
      handler() {
        this.sortPallets();
      },
      deep: true,
    },
    trailerLength() {
      this.calculateNumberOfPallets();
    },
    trailerWidth() {
      this.calculateNumberOfPallets();
    },
  },
  computed: {
    passHeight() {
      return !this.rotate ? this.trailerWidth : this.trailerLength;
    },
    passWidth() {
      return !this.rotate ? this.trailerLength : this.trailerWidth;
    },
  },
};
</script>

<style scoped>
.trailer {
  position: relative;
  background-color: #615244;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  align-content: flex-start;
  flex-wrap: wrap;
  flex-flow: column wrap;
  border: 0.08rem solid rgb(37, 31, 25);
  border-radius: 0.08rem;
  padding: 1px;
  overflow: hidden;
  /* transition: all 0.5s ease; */
}

.unloading {
  position: absolute;
  right: 0.1rem;
  bottom: 0.05rem;
  animation: blink 2s infinite 1s;
  color: #efefef;
}

@keyframes blink {
  0% {
    opacity: 100%;
  }
  50% {
    opacity: 0%;
  }
  100% {
    opacity: 100%;
  }
}

.pallet {
  border: 1px solid rgb(97, 82, 68);
  border-radius: 0.05rem;
  box-sizing: border-box;
  z-index: inherit;
  margin: auto;
  transition: all 0.2s ease;
}

.pallet::after {
  display: block;
  content: "";
  border-radius: 0.03rem;
  background: radial-gradient(
    circle,
    rgba(223, 163, 108, 0) 0%,
    rgba(0, 0, 0, 0.30015756302521013) 100%
  );
  width: 100%;
  height: 100%;
}

@media screen and (max-width: 1000px) {
  .trailer {
    position: relative;
    background-color: rgb(97, 82, 68);
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;
    align-content: flex-start;
    flex-wrap: wrap;
    border: 0.08rem solid rgb(37, 31, 25);
  }
  .unloading {
    left: 0.1rem;
  }
}
</style>
