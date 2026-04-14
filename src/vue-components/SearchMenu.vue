<script lang="ts">
export default {
  name: "SearchMenu",
  data() {
    return {
      isOpen: false,
      searchQuery: '',
    };
  },
  methods: {
    toggleSearch() {
      this.isOpen = !this.isOpen;
      if (this.isOpen) {
        this.$nextTick(() => {
          (this.$refs.searchInput as HTMLInputElement)?.focus();
        });
      } else {
        this.searchQuery = '';
      }
    },
    handleSearch() {
      if (this.searchQuery.trim()) {
        this.$emit('search', this.searchQuery.trim());
      }
    },
    closeSearch() {
      this.isOpen = false;
      this.searchQuery = '';
    },
  },
};
</script>

<template>
  <div class="mm-search" :class="{ open: isOpen }">
    <div class="mm-search-input-wrapper">
      <input
        ref="searchInput"
        v-model="searchQuery"
        type="text"
        class="mm-search-input"
        placeholder="Search..."
        @keyup.enter="handleSearch"
        @keyup.esc="closeSearch"
      />
    </div>
    <button class="mm-search-btn" @click="toggleSearch" :aria-label="isOpen ? 'Close search' : 'Open search'">
      <i class="fas fa-search"></i>
    </button>
  </div>
</template>
