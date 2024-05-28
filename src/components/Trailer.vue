<template>
  <div
    class="trailer"
    :style="{ height: trailerWidth + 'rem', width: trailerLength + 'rem' }"
  >
    <pallet
      v-for="(pallet, index) in pallets"
      key="index"
      :style="{ height: pallet.width + 'rem', width: pallet.length + 'rem' }"
      class="pallet"
      :palletNumber="index"
    ></pallet>
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
  background-color: rgb(223, 163, 108);
  border: 0.01rem solid rgb(97, 82, 68);
  box-sizing: border-box;
  z-index: inherit;
  margin: auto;
}
</style>
