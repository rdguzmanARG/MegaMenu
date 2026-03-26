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

<template>
  <div @mouseenter="toggleMenu" class="mmenu-item-desktop mm-icon">
    <i :class="iconClass"></i>
    <span>{{ label }}</span>
  </div>
  <div @click="toggleMenu" class="mmenu-item-mobile mm-icon">
    <i :class="iconClass"></i>
    <span>{{ label }}</span>
  </div>
</template>

<style>
.mmenu-item-mobile {
  display: none;
}

.mmenu-item-desktop {
  display: flex;
}

@media screen and (max-width: 1024px) {
  .mmenu-item-mobile {
    display: flex;
  }

  .mmenu-item-desktop {
    display: none;
  }
}

.mm-icon {
  cursor: pointer;
  width: fit-content;
  padding: 10px;
  color: #666666;
  gap: 10px;
  position: relative;
  align-items: center;

  &.home {
    color: #ff8300;
  }
}
</style>
