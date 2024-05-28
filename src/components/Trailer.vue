<template>
  <div
    class="trailer"
    :style="{ height: size.width + 'rem', width: size.length + 'rem' }"
  >
    <div
      v-for="pallet in pallets"
      :style="{ height: pallet.width + 'rem', width: pallet.length + 'rem' }"
      class="pallet"
    ></div>
  </div>
</template>

<script>
import Pallet from "./Pallet.vue";

export default {
  components: {
    Pallet,
  },
  props: ["size", "pallets"],
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
  gap: 1px;
}
.pallet {
  background-color: rgb(223, 163, 108);
  border: 0.06rem solid rgb(173, 117, 65);
  box-sizing: border-box;
  z-index: inherit;
  margin: auto;
}
</style>
