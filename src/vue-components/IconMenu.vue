<script lang="ts">
export default {
  name: "IconMenu",
  props: {
    iconClass: { type: String, required: false },
    iconUrl: { type: String, required: false },
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
    <div class="mm-icon-svg" v-if="iconUrl" :style="{ backgroundImage: `url(${iconUrl})` }"></div>
    <i v-else :class="iconClass"></i>
    <a :href="url" target="_blank">{{ label }}</a>
  </div>
  <div @click="toggleMenu" class="mm-item-mobile mm-icon">
    <div class="mm-icon-svg" v-if="iconUrl" :style="{ backgroundImage: `url(${iconUrl})` }"></div>
    <i v-else :class="iconClass"></i>
    <a :href="url" target="_blank">{{ label }}</a>
  </div>
</template>
