<template>
  <div @click="toggleMenu" class="mmenu-item mm-icon">
    <i :class="iconClass"></i>
    <span>{{ label }}</span>
  </div>
</template>

<script setup lang="TS">
export default {
  name: "IconMenu",
  props: {
    iconClass: { type: String, required: true },
    label: { type: String, required: true },
  },
  methods: {
    toggleMenu() {
      this.$emit("toggle", this.label); // notify parent which menu was clicked
    }
  }
};
</script>


<style scoped></style>
