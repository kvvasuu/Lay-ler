<template>
  <div
    class="trailer"
    :style="{ height: trailerWidth + 'rem', width: trailerLength + 'rem' }"
  >
    <TransitionGroup name="fade">
      <pallet
        v-for="(pallet, index) in pallets"
        key="index"
        :style="{ height: pallet.width + 'rem', width: pallet.length + 'rem' }"
        class="pallet"
        :palletNumber="index"
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
  props: ["trailerLength", "trailerWidth", "pallets"],
  data() {
    return {};
  },
};
</script>

<style scoped>
.trailer {
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
}
.pallet {
  transition: all 0.3s ease;
  background: rgb(223, 163, 108);
  background: radial-gradient(
    circle,
    rgba(223, 163, 108, 1) 0%,
    rgb(177, 126, 82) 100%
  );
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
