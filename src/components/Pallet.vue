<template>
  <div class="number">{{ palletNumber + 1 }}</div>
</template>

<script>
export default {
  props: ["palletNumber"],
  data() {
    return {};
  },
};
</script>

<style scoped>
.number {
  color: rgb(235, 235, 235);
  padding: 2px;
}
</style>
