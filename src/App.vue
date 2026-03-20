<!-- App.vue -->
<template>
  <MegaMenu :items="data" />
</template>

<script setup lang="ts">
import MegaMenu, { type MenuItem } from './MegaMenu.vue';

const data: MenuItem[] = [
  { text: 'Home', iconClass: 'fas fa-home' },
  {
    text: 'Services', items: [
      { text: 'Web Development', url: '/services/web' },
      { text: 'Mobile Development', url: '/services/mobile' },
      { text: 'SEO Optimization', url: '/services/seo' }
    ]
  },
  {
    text: 'Products', items: [
      {
        text: 'Laptops', url: '/products/laptops', items: [
          {
            text: 'Gaming Laptops', url: '/products/laptops/gaming', items: [
              { text: 'Alienware', url: '/products/laptops/gaming/alienware' },
              { text: 'ASUS ROG', url: '/products/laptops/gaming/asus-rog' },
              { text: 'MSI', url: '/products/laptops/gaming/msi' },
              { text: 'Lenovo Legion', url: '/products/laptops/gaming/lenovo-legion' }
            ]
          },
          {
            text: 'Ultrabooks', url: '/products/laptops/ultrabooks', items: [
              { text: 'Dell XPS', url: '/products/laptops/ultrabooks/dell-xps' },
              { text: 'HP Spectre', url: '/products/laptops/ultrabooks/hp-spectre' },
              { text: 'MacBook Air', url: '/products/laptops/ultrabooks/macbook-air' },
              { text: 'ASUS ZenBook', url: '/products/laptops/ultrabooks/asus-zenbook' }
            ]
          },
          {
            text: 'Business Laptops', url: '/products/laptops/business', items: [
              { text: 'Lenovo ThinkPad que pasa si es mucho mas largo', url: '/products/laptops/business/lenovo-thinkpad' },
              { text: 'Dell Latitude', url: '/products/laptops/business/dell-latitude' },
              { text: 'HP EliteBook', url: '/products/laptops/business/hp-elitebook' },
              { text: 'Microsoft Surface', url: '/products/laptops/business/microsoft-surface' }
            ]
          }
        ]
      },
      {
        text: 'Smartphones', url: '/products/smartphones', items: [
          { text: 'Android Phones', url: '/products/smartphones/android' },
          { text: 'iPhones', url: '/products/smartphones/iphone' }
        ]
      },
      {
        text: 'Accessories', url: '/products/accessories', items: [
          { text: 'Headphones', url: '/products/accessories/headphones' },
          { text: 'Chargers', url: '/products/accessories/chargers' }
        ]
      }
    ]
  },
  { text: 'Contact', url: '/contact', iconClass: 'fas fa-envelope' }
];
</script>
