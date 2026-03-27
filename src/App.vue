<script setup lang="ts">
import MegaMenu, { type MenuItem } from './vue-components/MegaMenu.vue';

const data: MenuItem[] = [
  { text: 'Home', url: '#', iconClass: 'fas fa-home' },
  {
    text: 'Gestión', items: [
      {
        text: 'Ternium',
        iconClass: 'fas fa-signal',
        items: [
          {
            text: 'Visual Flash', items: [
              { text: 'Ternium', url: '#' },
              { text: 'Argentina', url: '#' },
              { text: 'Brasil', url: '#' },
              { text: 'Internacional', url: '#' },
              { text: 'México', url: '#' }
            ]
          },
          {
            text: 'Site', items: [
              { text: 'Centroamerica', url: '#' },
              { text: 'Solombia', url: '#' },
              { text: 'USA', url: '#' },
            ]
          },
        ]
      }, {
        text: 'Seguridad y Medio Ambiente',
        iconClass: 'fas fa-hard-hat',
        items: [
          { text: 'Gestional', items: [{ text: "Site seguridad", url: '#' }, { text: "Site medio ambiente", url: '#' }] },
          { text: 'Transaccional', url: '#' },
          { text: 'Política y Procedimientos', url: '#' }
        ]
      }, { text: 'Industrial', iconClass: 'fas fa-industry', url: '/services/seo' },
      { text: 'Negocios', iconClass: 'fas fa-briefcase', url: '/services/seo' },
      { text: 'Calidad', iconClass: 'fas fa-star', url: '/services/seo' },
      { text: 'Planeamiento', iconClass: 'fas fa-chart-line', url: '/services/seo' }
    ]
  },
  {
    text: 'Servicios',
    items: [
      {
        text: 'Información', iconClass: 'fas fa-info', url: '#'
      }, {
        text: 'Utilidades', iconClass: 'fas fa-wrench', url: '#'
      }, {
        text: 'Enlaces', iconClass: 'fas fa-external-link-alt', url: '#'
      }]
  }, { text: 'Mi RH', iconClass: 'far fa-address-card', url: '#' }];
</script>

<template>
  <MegaMenu :items="data" />
</template>
