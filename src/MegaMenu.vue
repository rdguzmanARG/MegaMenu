<script lang="ts">
import DropdownMenu from "./DropdownMenu.vue";
import IconMenu from "./IconMenu.vue";
export interface MenuItem {
  text: string;
  url?: string;
  iconClass?: string;
  items?: MenuItem[];
}
export default {
  components: { DropdownMenu, IconMenu },
  props: ['items'],
  data() {
    return {
      openMenu: null as string | null, // tracks which menu is open
    };
  },
  created() {
    // Log props and data when the component is created
    console.log('Component data:', this.items);
  },
  methods: {
    handleToggle(label: string) {
      // if the same menu is clicked, close it; otherwise open the new one
      this.openMenu = this.openMenu === label ? null : label;
      console.log(`Toggled menu: ${label}, openMenu is now: ${this.openMenu}`);
    },
    handleSelection({ label, value }: { label: string; value: string; }) {
      console.log(`Selected "${value}" from "${label}"`);
      this.openMenu = null; // close after selection
    },
    close() {
      this.openMenu = null; // close after selection
    }
  }
};
</script>


<template>
  <div class="mm-container">
    <div class="mm-content">
      <div v-for="item in items" :key="item.text">
        <DropdownMenu v-if="item.items && item.items.length > 0" :label="item.text" :items="item.items"
          :isOpen="openMenu === item.text" @toggle="handleToggle" @item-selected="handleSelection" />
        <IconMenu v-else :label="item.text" :iconClass="item.iconClass" @toggle="handleToggle" />
      </div>
    </div>
  </div>
  <div @mouseenter="close" v-show="openMenu !== null" class=" mm-backround">
  </div>
</template>

<style scoped>
.mm-container {
  z-index: 10;
  height: 40px;
  box-shadow:
    0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 3px 1px -2px rgba(0, 0, 0, 0.2),
    0 1px 5px 0 rgba(0, 0, 0, 0.12);
  background-color: #f6f6f699 !important;
  border-bottom: solid 1px #e7e7e7;

  .mm-content {
    display: flex;
    padding: 0 19px;
    max-width: 1300px;
    width: 100%;
    margin: 0 auto;
    position: relative;
  }

  .mm-icon {
    cursor: pointer;
    width: fit-content;
    padding: 10px 15px;
    color: #666666;
    display: flex;
    gap: 10px;
    position: relative;

    &.home {
      color: #ff8300;
    }
  }
}

.mm-backround {
  background-color: rgba(0, 0, 0, 0.01);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -10;
}
</style>