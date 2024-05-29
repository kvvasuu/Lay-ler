<template>
  <div
    class="outer-pallet"
    :style="{
      height: pallet.width + 'rem',
      width: pallet.length + 'rem',
    }"
  >
    <div
      class="inner-pallet"
      :style="{
        height: pallet.width + 'rem',
        width: pallet.length + 'rem',
      }"
      @click="togglePalletSizeModal"
    >
      <div class="number">{{ pallet.number + 1 }}</div>
      <div class="name">
        {{ showName }}
      </div>
      <div class="dimensions">
        {{ showDimensions }}
      </div>
    </div>
    <transition name="fade">
      <pallet-modal
        v-if="showPalletSizeModal"
        :pallet="pallet"
        :palletNum="pallet.numer"
        @toggle-modal="togglePalletSizeModal"
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
  emits: ["sort", "update-pallet"],
  data() {
    return {
      showPalletSizeModal: false,
    };
  },
  methods: {
    togglePalletSizeModal() {
      this.showPalletSizeModal = !this.showPalletSizeModal;
    },
    /* updatePallet(pallet) {
      this.$emit("update-pallet", pallet);
    }, */
  },
  computed: {
    showDimensions() {
      if (this.pallet.length >= 0.8 && this.pallet.width >= 0.8) {
        return `${Number(this.pallet.length).toFixed(2)} x ${Number(
          this.pallet.width
        ).toFixed(2)}`;
      }
    },
    showName() {
      if (this.pallet.length >= 0.8 && this.pallet.width >= 0.8) {
        return this.pallet.name;
      }
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
