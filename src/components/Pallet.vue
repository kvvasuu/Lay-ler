<template>
  <div>
    <label for="width">Width:</label><input name="width" type="number" />
    <label for="length">Length:</label><input name="length" type="number" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      trailerSize: { x: 13.6, y: 2.45 },
      pallets: [],
    };
  },
  methods: {
    addPallet(x, y) {
      this.pallets.push({ x: x, y: y });
    },
  },
};
</script>

<style scoped></style>
