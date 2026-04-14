<script lang="ts">
import DropdownMenu from "./DropdownMenu.vue";
import IconMenu from "./IconMenu.vue";
import SearchMenu from "./SearchMenu.vue";
import "./mega-menu.scss";

export interface MenuItem {
  text: string;
  url?: string;
  iconClass?: string;
  iconUrl?: string;
  items?: MenuItem[];
}
export default {
  components: { DropdownMenu, IconMenu, SearchMenu },
  props: ['items', 'title', 'logoUrl'],
  data() {
    return {
      openMenu: null as string | null,
      mobileMenuIsOpen: false as boolean,
    };
  },
  created() {
    // Log props and data when the component is created
    // console.log('Component data:', this.items);
  },
  methods: {
    handleOpen(label: string) {
      // if the same menu is clicked, close it; otherwise open the new one
      this.openMenu = label;
    },
    handleToggle(label: string) {
      // if the same menu is clicked, close it; otherwise open the new one
      this.openMenu = this.openMenu === label ? null : label;
      console.log(`Toggled menu: ${label}, openMenu is now: ${this.openMenu}`);
    },
    handleSelection({ label, value }: { label: string; value: string; }) {
      console.log(`Selected "${value}" from "${label}"`);
      //this.openMenu = null; // close after selection
    },
    handleSearch(query: string) {
      console.log(`Search query: "${query}"`);
    },
    close() {
      this.openMenu = null; // close after selection
      this.mobileMenuIsOpen = false;
    },
    openMobileMenu() {
      this.mobileMenuIsOpen = !this.mobileMenuIsOpen;
    }
  }
};
</script>

<template>
  <div class="mm-container" :class="{ active: mobileMenuIsOpen }">
    <div class="mm-content" :class="{ active: mobileMenuIsOpen }">
      <div v-for="item in items" :key="item.text">
        <DropdownMenu v-if="item.items && item.items.length > 0" :label="item.text" :items="item.items"
          :isOpen="openMenu === item.text" @open="handleOpen" @toggle="handleToggle" @item-selected="handleSelection" />
        <IconMenu v-else :url="item.url" :label="item.text" :iconUrl="item.iconUrl" :iconClass="item.iconClass"
          @toggle="handleToggle" />
      </div>
      <SearchMenu @search="handleSearch" />
    </div>
    <div class="mm-hamburger" :class="{ active: mobileMenuIsOpen }" @click="openMobileMenu">
      <span></span>
    </div>
    <div class="mm-hamburger-title">
      <img v-if="logoUrl" :src="logoUrl" class="mm-logo" alt="Logo" />
      <span v-if="title">{{ title }}</span>
    </div>
  </div>
  <div @mouseenter="close" v-show="openMenu !== null" class="mm-backround">
  </div>
</template>
