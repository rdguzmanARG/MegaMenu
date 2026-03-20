<template>
  <div @click="toggleMenu" class="mmenu-item mm-icon">
    <i :class="iconClass"></i>
    <span>{{ label }}</span>
  </div>
</template>

<script lang="ts">
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


<style scoped>
.mm-icon {
  cursor: pointer;
  width: fit-content;
  padding: 10px 15px;
  color: #666666;
  display: flex;
  gap: 10px;
  position: relative;
  display: flex;
  align-items: center;

  &.home {
    color: #ff8300;
  }
}
</style>
