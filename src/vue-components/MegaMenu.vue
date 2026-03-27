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
        <IconMenu v-else :url="item.url" :label="item.text" :iconClass="item.iconClass" @toggle="handleToggle" />
      </div>
    </div>
    <div class="mm-hamburger" :class="{ active: mobileMenuIsOpen }" @click="openMobileMenu">
      <span></span>
    </div>
    <div class="mm-hamburger-title">Intra - Ternium</div>
  </div>
  <div @mouseenter="close" v-show="openMenu !== null" class="mm-backround">
  </div>
</template>

<style>
.mm-container {
  z-index: 10;
  height: 40px;
  box-shadow:
    0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 3px 1px -2px rgba(0, 0, 0, 0.2),
    0 1px 5px 0 rgba(0, 0, 0, 0.12);
  background-color: #f6f6f699;
  border-bottom: solid 1px #e7e7e7;
  display: flex;

  @media screen and (max-width: 1024px) {
    height: 48px;

    &.active {
      height: 100vh;
      background-color: #e8e8e8 !important;

      .mm-hamburger-title {
        display: none;
      }
    }
  }

  .mm-content {
    display: flex;
    padding: 0 19px;
    max-width: 1300px;
    width: 100%;
    margin: 0 auto;
    position: relative;

    @media screen and (max-width: 1024px) {
      display: none;
      flex-direction: column;
      background-color: #f6f6f699;


      &.active {
        display: flex;
        padding: 0;
      }
    }
  }

  .mm-hamburger {
    display: none;

    /* Hide hamburger on larger screens */
    @media screen and (max-width: 1024px) {
      position: relative;
      width: 30px;
      height: 20px;
      /* Adjust height to give space for bars */
      cursor: pointer;
      display: block;
      margin: 10px;

      span {

        position: absolute;
        top: 10px;
        cursor: pointer;
        height: 2px;
        width: 17px;
        background: #9196a0;
        display: block;
        content: '';
        transition: all 500ms ease-in-out;

        &::before {
          cursor: pointer;
          height: 2px;
          width: 17px;
          background: #9196a0;
          position: absolute;
          display: block;
          content: '';
          top: -7px;
          width: 23px;
          transition: all 500ms ease-in-out;
        }

        &::after {
          cursor: pointer;
          height: 2px;
          width: 17px;
          background: #9196a0;
          position: absolute;
          display: block;
          content: '';
          bottom: -7px;
          width: 17px;
          transition: all 500ms ease-in-out;
        }
      }

      &.active span {
        background: transparent;

        &::before {
          transform: rotate(45deg);
          top: 0;
          transition: all 500ms ease-in-out;
        }

        &::after {
          transform: rotate(-45deg);
          bottom: 0;
          width: 23px;
          transition: all 500ms ease-in-out;
        }

      }
    }
  }

  .mm-hamburger-title {
    display: none;

    @media screen and (max-width: 1024px) {
      display: inline-block;
      width: fit-content;
      margin: 10px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }


}

@media screen and (min-width: 1024px) {
  .mm-backround {
    background-color: rgba(0, 0, 0, 0.01);
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -10;
  }
}
</style>