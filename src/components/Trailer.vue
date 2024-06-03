<template>
  <div
    class="trailer"
    :style="{ height: trailerWidth + 'rem', width: trailerLength + 'rem' }"
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
      ></pallet>
    </TransitionGroup>
  </div>
</template>

<script>
import Pallet from "./Pallet.vue";

export default {
  components: {
    Pallet,
  },
  props: ["trailerLength", "trailerWidth", "pallets", "sort"],
  emits: ["update-all-pallets"],
  data() {
    return {
      lastPalletPosition: 0,
      trailerRightBorderPosition: 0,
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
      this.trailerRightBorderPosition =
        this.$refs.trailer.getBoundingClientRect().right;

      setTimeout(() => {
        this.lastPalletPosition =
          this.$refs.pallets[
            this.$refs.pallets.length - 1
          ].$el.getBoundingClientRect().right;

        if (this.lastPalletPosition > this.trailerRightBorderPosition) {
          this.pallets.pop();
          this.calculateNumberOfPallets();
        }
      }, 600);
    },
    updateAllPallets(pallet) {
      this.pallets.map((el) => {
        el.length = pallet.length;
        el.width = pallet.width;
        el.name = pallet.name;
        el.color = pallet.color;
      });
    },
  },
  watch: {
    sort() {
      this.sortPallets();
    },
  },
};
</script>

<style scoped>
.trailer {
  position: relative;
  background-color: rgb(97, 82, 68);
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
}

.pallet {
  transition: all 0.3s ease;
  border: 1px solid rgb(97, 82, 68);
  border-radius: 0.05rem;
  box-sizing: border-box;
  z-index: inherit;
  margin: auto;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
