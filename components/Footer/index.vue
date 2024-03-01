<template>
  <div class="bg-gray-800 py-5 text-center text-white absolute bottom-0 w-full">
    <div class="flex flex-col">
      <span>
        Made with ❤️
        <!-- <Icon name="material-symbols:favorite-rounded" color="crimson" /> -->
        © 2023 -{{ date }} All rights reserved.
      </span>
      
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const date = ref(new Date().getFullYear());
</script>

<style scoped>
/* Add your component-specific styles here */
</style>
