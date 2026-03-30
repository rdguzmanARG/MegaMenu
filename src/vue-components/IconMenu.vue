<script lang="ts">
export default {
  name: "IconMenu",
  props: {
    iconClass: { type: String, required: true },
    label: { type: String, required: true },
    url: { type: String, required: true },
  },
  methods: {
    toggleMenu() {
      this.$emit("toggle", this.label); // notify parent which menu was clicked
    }
  }
};
</script>

<template>
  <div @mouseenter="toggleMenu" class="mm-item-desktop mm-icon">
    <i :class="iconClass"></i>
    <a :href="url" target="_blank">{{ label }}</a>
  </div>
  <div @click="toggleMenu" class="mm-item-mobile mm-icon">
    <i :class="iconClass"></i>
    <a :href="url" target="_blank">{{ label }}</a>
  </div>
</template>
