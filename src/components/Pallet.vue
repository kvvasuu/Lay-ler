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
      <div class="number" :style="{ color: textColor }">
        {{ pallet.number + 1 }}
      </div>
      <div class="name" :style="{ color: textColor }">
        {{ showName }}
      </div>
      <div class="dimensions" :style="{ color: textColor }">
        {{ showDimensions }}
      </div>
    </div>
    <transition name="fade">
      <pallet-modal
        v-if="showPalletSizeModal"
        :pallet="pallet"
        @toggle-modal="togglePalletSizeModal"
        @update-pallet="updatePallet"
        @update-all-pallets="(pallet) => $emit('update-all-pallets', pallet)"
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
  props: ["pallet"],
  emits: ["sort", "update-pallet", "update-all-pallets"],
  data() {
    return {
      showPalletSizeModal: false,
      textColor: "#efefef",
    };
  },
  methods: {
    togglePalletSizeModal() {
      this.showPalletSizeModal = !this.showPalletSizeModal;
    },
    updatePallet() {
      this.$emit("update-pallet");
    },
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
  padding: 2px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.dimensions {
  position: absolute;
  font-size: 0.16rem;
  bottom: 0.05rem;
  right: 0.05rem;
  opacity: 0;
  z-index: 2;
  transition: opacity 0.3s ease;
}

.name {
  position: absolute;
  font-size: 0.16rem;
  text-align: center;
  left: 5%;
  top: 50%;
  transform: translate(0, -50%);
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
