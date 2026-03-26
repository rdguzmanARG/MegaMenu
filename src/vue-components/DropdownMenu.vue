<script lang="ts">
import type { PropType } from 'vue';

interface MenuItem {
  text: string;
  iconClass?: string;
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
    <span class="mm-options-desktop" @mouseenter="openMenu(); selectedIndex = null;">{{ label
    }}</span>
    <span class="mm-options-mobile" @click="toggleMenu(); selectedIndex = null;">{{ label
    }}</span>
    <div v-if="isOpen" class="mm-options-container">
      <div class="mm-options-desktop-n1" @mouseenter="openSelection(index)"
        :class="{ 'selected': selectedIndex === index }" v-for="(item, index) in items" :key="index">
        <i :class="item.iconClass"></i>
        <div class="mm-options-n1-title">{{ item.text }}</div>
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
      <div class="mm-options-mobile-n1" @click="toggleSelection(index)" :class="{ 'selected': selectedIndex === index }"
        v-for="(item, index) in items" :key="index">

        <div class="mm-options-n1-title"><i :class="item.iconClass"></i> {{ item.text }}</div>
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

<style>
@media screen and (max-width: 1024px) {
  .active {
    .mm-options {
      background-color: #e8e8e8;

      border-bottom: rgba(0, 0, 0, 0.13) 1px solid;

      .mm-options-mobile {
        justify-content: space-between;
        padding: 10px;
      }
    }
  }
}

.mm-options {
  cursor: pointer;
  width: fit-content;
  width: 100%;
  display: inline-block;
  cursor: pointer;

  color: #666666;

  span::after {
    color: #7b7b7b;
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
  }
}

.mm-options-desktop {
  display: flex;
  padding: 10px;
  align-items: center;
  gap: 10px;
  text-transform: uppercase;

  @media screen and (max-width: 1024px) {
    display: none;
  }
}

.mm-options-mobile {
  display: none;
  text-transform: uppercase;


  @media screen and (max-width: 1024px) {
    display: flex;
    align-items: center;
  }
}

.mm-options-container {
  position: absolute;
  top: 41px;
  left: 0;
  right: 0;
  background: #fff;
  border: 1px solid #ccc;
  min-width: 150px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);

  @media screen and (max-width: 1024px) {
    box-shadow: unset;
    position: relative;
    top: 0;
    border: none;
  }
}

.mm-options-mobile-n1 {
  display: none;
}

.mm-options-desktop-n1 {
  display: flex;
}

@media screen and (max-width: 1024px) {
  .mm-options-mobile-n1 {
    display: flex;
  }

  .mm-options-desktop-n1 {
    display: none;
  }
}

.mm-options-desktop-n1,
.mm-options-mobile-n1 {
  padding: 12px;
  gap: 10px;
  width: 25%;

  @media screen and (max-width: 1024px) {
    width: unset;
  }

  &.selected {
    border-left: 3px solid #ffa626;
    background: #f0f0f0;

    @media screen and (max-width: 1024px) {
      flex-direction: column;
      border-left: unset
    }

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
  width: 73%;
  color: #333;
  cursor: pointer;
  display: none;
  position: absolute;
  top: 0;
  height: calc(100vh - 70px);
  left: 26%;
  padding: 10px;
  flex-wrap: wrap;
  gap: 20px;

  @media screen and (max-width: 1024px) {
    position: relative;
    height: unset;
    width: unset;
    left: unset;
  }
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
