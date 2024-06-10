<template>
  <div @click="() => this.$emit('toggle-modal')" class="help-modal">
    <div class="help-modal-content" @click.stop="">
      <div class="close" @click="() => this.$emit('toggle-modal')"></div>
      <div class="title">How to use?</div>
      <div class="content"></div>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["toggle-modal"],
};
</script>

<style scoped>
.help-modal {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 99;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.185);
  backdrop-filter: blur(2px);
  cursor: default;
}

.help-modal-content {
  position: inherit;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-direction: column;
  height: 10rem;
  width: 10rem;
  margin: 0.2rem;
  border-radius: 0.6rem;
  background-color: rgba(54, 54, 56, 0.842);
}

.title {
  font-size: 0.7rem;
  font-weight: bold;
  margin: 1.3rem 0.6rem 0rem 0.6rem;
  width: 5rem;
  text-align: center;
}

.close {
  position: absolute;
  right: 0.5rem;
  top: 0.5rem;
  width: 0.5rem;
  height: 0.5rem;
  z-index: 10;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close:hover {
  filter: drop-shadow(0 0 6px rgb(160, 113, 70));
}

.close:hover:before,
.close:hover:after {
  background-color: rgb(241, 165, 94);
}

.close:before,
.close:after {
  position: absolute;
  left: 0.2rem;
  content: " ";
  height: 0.6rem;
  width: 0.06rem;
  background-color: #bdbdbd;
}
.close:before {
  transform: rotate(45deg);
}
.close:after {
  transform: rotate(-45deg);
}

@media screen and (max-width: 1000px) {
  .help-modal-content {
    width: auto;
  }
  .title {
    width: auto;
  }
}
</style>
