<template>
  <div class="layout6">
  <ClientOnly>
    <div class="pr-5 z-100">
      <Drawer />
    </div>
    
    
      <!-- Main Content -->
      <div class="mx-auto px-4 py-8">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <!-- First column -->
          <div>
            <img :src="data.thumbnail" alt="Thumbnail" class="w-full h-auto" />
          </div>

          <!-- Second column -->
          <div class="">
           n;bjnwcl=kbnw=ldkfnb
          </div>
        </div>


      </div>


    
    

    <ShareButtons />
  
    <!-- SEO metadata -->
    <!-- Regular Meta Tags -->
    <Title>{{ data.title }}</Title>
    <Meta name="description" :content="data.description" />
    <Meta name="tags" :content="data.tags" />
    <Meta name="keywords" :content="data.tags.join(', ')" /> <!-- Add keywords here -->
    <!-- Open Graph Meta Tags -->
    <Meta property="og:title" :content="data.title" />
    <Meta property="og:description" :content="data.description" />
    <Meta property="og:image" :content="data.thumbnail" /> <!-- Add an image URL here -->
    <Meta property="og:url" :content="data.url" /> <!-- Add the current URL -->
    <Meta property="og:type" content="article" /> 
  
  </ClientOnly>
  </div>
</template>

<script setup>
defineProps(['data', 'formatDate']);
</script>
