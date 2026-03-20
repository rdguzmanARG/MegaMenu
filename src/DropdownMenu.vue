<template>
  <div class="mm-options">
    <span @click="toggleMenu">{{ label }}</span>
    <div v-if="isOpen" class="mm-options-container">
      <div class="mm-options-n1" @mouseover="hoveredIndex = index" :class="{ 'hovered': hoveredIndex === index }"
        v-for="(item, index) in items" :key="index" @click.stop="selectItem(item)">
        <div class="mm-options-n1-title">{{ item.text }}</div>
        <div class="mm-options-n1-container">

          <div class="mm-options-n2" v-for="(subitem, index) in item.items" :key="index">
            <div class="mm-options-n2-title">{{ subitem.text }}</div>
            <div class="mm-options-n2-container">
              <div v-for="(subitem2, index) in subitem.items" :key="index">
                <div class="mm-options-n3-text">{{ subitem2.text }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import type { PropType } from 'vue';

interface MenuItem {
  text: string;
  items?: MenuItem[];
}

export default {
  name: "DropdownMenu",
  props: {
    label: { type: String, required: true },
    items: { type: Array as PropType<MenuItem[]>, default: () => [] },
    isOpen: { type: Boolean, default: false } // controlled by parent
  },
  data() {
    return {
      hoveredIndex: null as number | null
    };
  },
  methods: {
    toggleSubmenu() {

      console.log("Toggle submenu");
    },
    toggleMenu() {
      this.$emit("toggle", this.label); // notify parent which menu was clicked
    },
    selectItem(item: MenuItem) {
      this.$emit("item-selected", { label: this.label, value: item });
    }
  }
};
</script>


<style scoped>
.mm-options {
  cursor: pointer;
  width: fit-content;

  display: inline-block;
  cursor: pointer;
  padding: 10px;
  color: #666666;
  padding-left: 20px;
  padding-right: 20px;

  span::after {
    color: #7b7b7b;
    display: inline-block;
    font-style: normal;
    font-variant: normal;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    content: '\f107';
    font-family: 'Font Awesome 5 Free';
    font-weight: 900;
    font-size: 18px;
    line-height: 10px;
    margin-left: 10px;
    position: relative;
    right: -7px;
    text-align: right;
    top: 3px;
  }
}

.mm-options-container {
  position: absolute;
  top: 42px;
  left: 0;
  right: 0;
  background: #fff;
  border: 1px solid #ccc;
  min-width: 150px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
}

.mm-options-n1 {
  padding: 12px;
  display: flex;
  gap: 10px;
  width: 25%;

  &.hovered {
    border-left: 3px solid #ffa626;
    background: #f0f0f0;

    .mm-options-n1-container {
      display: flex;
      background: #f0f0f0;

    }
  }
}

.mm-options-n1-title {
  color: #333;
  cursor: pointer;
}

.mm-options-n1-container {
  width: 75%;
  color: #333;
  cursor: pointer;
  display: none;
  position: absolute;
  top: 0;
  height: calc(100vh);
  left: 25%;
  padding: 10px;
  flex-wrap: wrap;
  gap: 20px;
}

.mm-options-n2 {
  padding: 10px;
}

.mm-options-n2-title {
  font-size: 14px;
  font-weight: bold;
  text-transform: uppercase;
}

.mm-options-n2-container {
  padding: 10px;
}

.mm-options-n3-text {
  text-wrap: wrap;
}
</style>
