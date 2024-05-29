<template>
  <div
    class="inner-pallet"
    @click="changePalletSize"
    :style="{ height: pallet.width + 'rem', width: pallet.length + 'rem' }"
  >
    <div class="number">{{ palletNumber + 1 }}</div>
    <div class="dimensions">{{ pallet.length }} x {{ pallet.width }}</div>
  </div>
</template>

<script>
export default {
  props: ["palletNumber", "pallet"],
  emits: ["sort"],
  data() {
    return {};
  },
  methods: {
    changePalletSize() {
      this.pallet.width = this.pallet.width + 0.2;
      this.pallet.length = this.pallet.length + 0.2;
      this.$emit("sort");
    },
  },
};
</script>

<style scoped>
.inner-pallet {
  position: relative;
  cursor: pointer;
}

.number {
  color: rgb(235, 235, 235);
  padding: 2px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.dimensions {
  color: rgb(235, 235, 235);
  position: absolute;
  font-size: 0.16rem;
  bottom: 0.05rem;
  right: 0.05rem;
  opacity: 0;
  z-index: 2;
  transition: opacity 0.3s ease;
}

.inner-pallet:hover .dimensions,
.inner-pallet:hover .number {
  opacity: 1;
}
</style>
