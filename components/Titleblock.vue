<template>
    <div class="opacity-80 animate-fade animate-once animate-delay-[100ms]">
      <div
      class="text-4xl font-bold text-amber-500">
      {{ settings.site_title }}</div>
      <div class="text-base opacity-80 mb-10">
      {{ settings.site_subtitle }}
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const settings = ref({});
  // Use useFetch to load the JSON data from the public folder
  const { data, error } = await useFetch('/_data/settings.json');
  
  // Set the data to the settings ref
  if (data.value) {
    settings.value = data.value;
  } else if (error.value) {
    console.error('Error loading settings:', error.value);
  }
  </script>