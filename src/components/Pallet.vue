<template>
  <div
    class="outer-pallet"
    :style="{
      height: passHeight + 'rem',
      width: passWidth + 'rem',
      'background-color': colorCompute,
    }"
    :class="{ 'hide-outer-pallet': hidePallet }"
    @drop="(event) => drop(event)"
    @dragover="(event) => allowDrop(event)"
    @dragenter="dragEnter"
    @dragleave="dragLeave"
    @dragend="dragEnd"
    :id="pallet.number"
  >
    <div
      draggable="true"
      @dragstart="(event) => drag(event)"
      class="inner-pallet"
      :style="{
        height: passHeight + 'rem',
        width: passWidth + 'rem',
      }"
      :class="{ 'hide-pallet': hidePallet }"
      @click="togglePalletSizeModal"
      :id="pallet.number + 'a'"
    >
      <div class="number">
        {{ pallet.number + 1 }}
      </div>
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
  props: ["pallet", "rotate"],
  emits: ["sort", "update-pallet", "update-all-pallets", "replace-pallets"],
  data() {
    return {
      showPalletSizeModal: false,
      hidePallet: false,
      dragCounter: 0,
    };
  },
  methods: {
    togglePalletSizeModal() {
      this.showPalletSizeModal = !this.showPalletSizeModal;
    },
    updatePallet() {
      this.$emit("update-pallet");
    },
    //drag and drop methods
    drag(event) {
      const palletNumber = event.target.id.replace("a", "");
      event.dataTransfer.setData("pallet", palletNumber);
    },
    dragEnter(event) {
      event.preventDefault();
      this.hidePallet = true;
      this.dragCounter++;
    },
    dragLeave(event) {
      event.preventDefault();
      this.dragCounter--;
      if (this.dragCounter === 0) this.hidePallet = false;
    },
    dragEnd(event) {
      event.preventDefault();
      this.hidePallet = false;
      this.dragCounter = 0;
    },
    allowDrop(event) {
      event.preventDefault();
    },
    drop(event) {
      event.preventDefault();
      event.stopPropagation();
      const data = event.dataTransfer.getData("pallet");
      this.replacePallet(data, event.target.id);
      this.hidePallet = false;
      this.dragCounter = 0;
      event.dataTransfer.clearData("pallet");
    },
    replacePallet(draggedPallet, palletToReplace) {
      if (draggedPallet === palletToReplace) {
        return;
      }
      this.$emit("replace-pallets", draggedPallet, palletToReplace);
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
    passHeight() {
      return !this.rotate ? this.pallet.width : this.pallet.length;
    },
    passWidth() {
      return !this.rotate ? this.pallet.length : this.pallet.width;
    },
    colorCompute() {
      return this.pallet.color;
    },
  },
};
</script>

<style scoped>
.outer-pallet {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.hide-outer-pallet {
  opacity: 0;
}

.inner-pallet {
  position: absolute;
  cursor: pointer;
  color: #efefef;
}

.hide-pallet {
  display: none;
}

.number {
  padding: 2px;
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
  z-index: 2;
  width: 90%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  transition: opacity 0.3s ease;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@media (hover: hover) {
  .inner-pallet:hover .dimensions,
  .inner-pallet:hover .number,
  .inner-pallet:hover .name {
    opacity: 1;
  }
  .number,
  .name {
    opacity: 0;
  }
}
</style>
