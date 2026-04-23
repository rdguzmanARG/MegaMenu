<script setup lang="ts">
import MegaMenu from './vue-components/MegaMenu.vue';
import FileUploadForm from './vue-components/FileUploadForm.vue';

import data from './data-tenaris.json';
//import data from './data-ternium.json';
</script>

<template>
  <MegaMenu :items="data.items" :title="data.title" :logoUrl="data.logoUrl" />
  <!-- <FileUploadForm /> -->
</template>
