<template>
  <div @click="() => this.$emit('toggle-modal')" class="help-modal">
    <div class="help-modal-content" @click.stop="">
      <div class="title">Are you sure?</div>
      <div class="buttons">
        <button class="button-set" @click="$emit('toggle-modal')">
          Cancel
        </button>
        <button class="button-delete" @click="deletePallet">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["toggle-modal", "delete-pallet"],
  methods: {
    deletePallet() {
      this.$emit("delete-pallet");
      this.$emit("toggle-modal");
    },
  },
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
  height: auto;
  width: auto;
  margin: 0.2rem;
  padding: 0.7rem;
  border-radius: 0.6rem;
  border: 2px solid #bdbdbd;
  background-color: rgba(54, 54, 56, 1);
}

.title {
  font-size: 0.6rem;
  text-align: center;
}

.buttons {
  display: flex;
  align-items: center;
  justify-content: center;
}

.button-set {
  background-color: #343435;
  cursor: pointer;
  border: 1px solid #bdbdbd;
  border-radius: 2rem;
  padding: 0.1rem;
  text-align: center;
  color: #bdbdbd;
  width: 1.4rem;
  font-size: 0.3rem;
  font-weight: bold;
  transition: color 0.3s ease, border 0.3s ease, filter 0.3s ease;
  margin: 0.4rem 0.2rem 0 0.2rem;
}

.button-delete {
  color: #cc3333;
  background-color: transparent;
  cursor: pointer;
  border: 1px solid #cc3333;
  border-radius: 2rem;
  padding: 0.1rem;
  text-align: center;
  width: 1.4rem;
  font-size: 0.3rem;
  font-weight: bold;
  transition: color 0.3s ease, border 0.3s ease, filter 0.3s ease;
  margin: 0.4rem 0.2rem 0 0.2rem;
}

.button-delete:hover {
  color: #cc3333;
  border: 1px solid #cc3333;
  filter: drop-shadow(0 0 1px #cc3333);
}

.button-set:hover {
  color: rgb(241, 165, 94);
  border: 1px solid rgb(241, 165, 94);
  filter: drop-shadow(0 0 1px rgb(160, 113, 70));
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
