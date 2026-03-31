<script lang="ts">
import type { PropType } from 'vue';

interface MenuItem {
  text: string;
  iconClass?: string;
  iconUrl?: string;
  url?: string;
  items?: MenuItem[];
}

export default {
  name: "DropdownMenu",
  props: {
    label: { type: String, required: true },
    items: { type: Array as PropType<MenuItem[]> },
    isOpen: { type: Boolean, default: false } // controlled by parent
  },
  data() {
    return {
      selectedIndex: null as number | null
    };
  },
  methods: {
    toggleSelection(label: number) {
      if (this.selectedIndex === label) {
        this.selectedIndex = null; // deselect if same item is clicked
      } else {
        this.selectedIndex = label; // select new item
      }
    },
    openSelection(label: number) {
      this.selectedIndex = label; // select new item
    },

    toggleMenu() {
      this.$emit("toggle", this.label); // notify parent which menu was clicked
    },
    openMenu() {
      this.$emit("open", this.label); // notify parent which menu was clicked
    },

  }
};
</script>

<template>
  <div class="mm-options">
    <span class="mm-options-desktop" @mouseenter="openMenu(); selectedIndex = 0;">{{ label
    }}</span>
    <span class="mm-options-mobile" @click="toggleMenu(); selectedIndex = null;">{{ label
    }}</span>
    <div v-if="isOpen" class="mm-options-container">
      <div class="mm-options-desktop-n1" @mouseenter="openSelection(index)"
        :class="{ 'selected': selectedIndex === index }" v-for="(item, index) in items" :key="index">
        <div class="mm-icon-svg" v-if="item.iconUrl" :style="{ backgroundImage: `url(${item.iconUrl})` }"></div>
        <i v-else :class="item.iconClass"></i>
        <div class="mm-options-n1-title">{{ item.text }}</div>
        <div v-if="item.items && item.items.length > 0" class="mm-options-n1-container">

          <div class="mm-options-n2" v-for="(subitem, index) in item.items" :key="index">
            <div v-if="subitem.items && subitem.items?.length > 0" class="mm-options-n2-title">{{ subitem.text }} 2
            </div>
            <a v-else class="mm-options-n2-link" :href="subitem.url" target="_blank">{{ subitem.text }}</a>

            <div v-if="subitem.items && subitem.items?.length > 0" class="mm-options-n2-container">
              <a v-for="(subitem2, index) in subitem.items" :key="index" class="mm-options-n3-link" :href="subitem2.url"
                target="_blank">{{ subitem2.text }}</a>
            </div>
          </div>
        </div>
      </div>
      <div class="mm-options-mobile-n1" @click="toggleSelection(index)" :class="{ 'selected': selectedIndex === index }"
        v-for="(item, index) in items" :key="index">

        <div class="mm-options-n1-title">
          <div class="mm-icon-svg" v-if="item.iconUrl" :style="{ backgroundImage: `url(${item.iconUrl})` }"></div>
          <i v-else :class="item.iconClass"></i> {{ item.text }}
        </div>
        <div v-if="item.items && item.items.length > 0" class="mm-options-n1-container">

          <div class="mm-options-n2" v-for="(subitem, index) in item.items" :key="index">
            <div class="mm-options-n2-title">{{ subitem.text }}</div>
            <div class="mm-options-n2-container">
              <div v-for="(subitem2, index) in subitem.items" :key="index">
                <a class="mm-options-n3-text" :href="subitem2.url" target="_blank">{{ subitem2.text }}</a>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>
