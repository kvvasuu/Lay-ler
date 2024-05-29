<template>
  <div
    class="outer-pallet"
    :style="{ height: palletWidth + 'rem', width: palletLength + 'rem' }"
  >
    <div
      class="inner-pallet"
      @click="togglePalletSizeModal"
      :style="{ height: palletWidth + 'rem', width: palletLength + 'rem' }"
    >
      <div class="number">{{ palletNumber + 1 }}</div>
      <div class="name">
        {{ palletName }}
      </div>
      <div class="dimensions">
        {{ Number(palletLength).toFixed(2) }} x
        {{ Number(palletWidth).toFixed(2) }}
      </div>
    </div>
    <transition name="fade">
      <pallet-modal
        v-if="showPalletSizeModal"
        :pallet="pallet"
        :palletNum="palletNumber"
        @toggle-modal="togglePalletSizeModal"
        @update-pallet="updatePallet"
      ></pallet-modal>
    </transition>
  </div>
</template>

<script>
import PalletModal from "./PalletModal.vue";

export default {
  components: {
    PalletModal,
  },
  props: ["palletNum", "pallet"],
  emits: ["sort"],
  data() {
    return {
      showPalletSizeModal: false,
      palletLength: this.pallet.length,
      palletWidth: this.pallet.width,
      palletName: "",
      palletNumber: this.palletNum,
    };
  },
  methods: {
    togglePalletSizeModal() {
      this.showPalletSizeModal = !this.showPalletSizeModal;
    },
    updatePallet(pallet) {
      this.palletLength = pallet.palletLength;
      this.palletWidth = pallet.palletWidth;
      this.palletNumber = pallet.palletNumber;
      this.palletName = pallet.palletName;
      this.$emit("sort");
    },
  },
};
</script>

<style scoped>
.outer-pallet {
  cursor: pointer;
}

.inner-pallet {
  position: absolute;
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

.name {
  color: rgb(235, 235, 235);
  position: absolute;
  font-size: 0.16rem;
  left: 50%;
  transform: translate(-50%, 0);
  opacity: 0;
  z-index: 2;
  width: 90%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  transition: opacity 0.3s ease;
}

.inner-pallet:hover .dimensions,
.inner-pallet:hover .number,
.inner-pallet:hover .name {
  opacity: 1;
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
