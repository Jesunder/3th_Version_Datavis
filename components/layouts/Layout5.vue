<template>
  <div class="layout5">
    <ClientOnly>
      <div class="relative max-h-screen overflow-hidden z-1">
        <!-- Lazy loading applied to NuxtImg -->
        <NuxtImg
          v-if="data.thumbnail"
          :src="data.thumbnail"
          class="w-full h-1/2 opacity-100 bg-cover mx-auto object-cover animate-move-left-right"
          :alt="`Thumbnail for ${data.title}`"
          format="webp"
          loading="lazy"
          @load="imageLoaded = true"
        />

        <div v-if="!imageLoaded" class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-75 z-2">
          <div class="spinner"></div>
        </div>

        <!-- Overlay content -->
        <div
          class="absolute inset-0 flex flex-col items-center justify-center bg-black bg-opacity-50 animate-fade animate-once animate-delay-[500ms] z-3"
          v-if="imageLoaded"
        >
          <div class="container p-4 text-center">
            <div>
              <h1 class="text-white text-4xl md:text-6xl lg:text-8xl font-bold">{{ data.title }}</h1>
              <h1 v-if="data.subtitle" class="text-white opacity-80 pt-3 text-xl md:text-2xl lg:text-3xl font-bold pb-10">{{ data.subtitle }}</h1>
            </div>

            <div>
              <p v-if="data.author" class="text-white opacity-80 text-xs font-bold">{{ data.author }}</p>
              <p class="text-white text-xs opacity-50 hover:opacity-100">Last update: {{ formatDate(data.date) }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Loading Drawer component -->
      <div class="absolute top-0 right-0 z-10 pr-5">
        <Drawer />
      </div>

      <!-- Main section -->
      <div class="container mx-auto p-10 animate-fade animate-once animate-delay-[500ms]" v-if="imageLoaded">
        <div class="grid grid-cols-1 md:grid-cols-1 gap-10 mt-1 max-w-4xl mx-auto">
          <!-- First column -->


          <!-- Second column -->
          <div class="flex flex-col justify-center">
            <ContentRenderer :value="data" />
          </div>
        </div>

        <!-- Second row -->
        <div v-if="data.related_page" class="mt-10">
          <RelatedPages :relatedPages="data.related_page" />
        </div>

        <!-- Link and published date -->
        <div class="text-xs leading-3 mt-6">
          <hr />
          <p class="text-xs opacity-50 hover:opacity-100 pb-2">Last update: {{ formatDate(data.date) }}</p>
          <article v-if="data.tags" class="tags">
            <li v-for="(item, index) in data.tags" :key="index" class="pt-2 text-xs opacity-50 hover:opacity-100">
              <NuxtLink :to="`/tags/${item}`">{{ item }}</NuxtLink>
            </li>
          </article>
        </div>
      </div>

      <!-- Loading the ShareButtons component -->
      <div class="mt-6">
        <ShareButtons />
      </div>

      <!-- SEO metadata -->
      <Title>{{ data.title }}</Title>
      <Meta name="description" :content="data.description" />
      <Meta name="tags" :content="data.tags.join(', ')" />
      <Meta name="keywords" :content="data.tags.join(', ')" />
      <Meta property="og:title" :content="data.title" />
      <Meta property="og:description" :content="data.description" />
      <Meta property="og:image" :content="data.thumbnail" />
      <Meta property="og:url" :content="data.url" />
      <Meta property="og:type" content="article" />
    </ClientOnly>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const imageLoaded = ref(false);

defineProps(['data', 'formatDate']);
</script>

<style scoped>
.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #fff;
  border-radius: 50%;
  width: 80px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@keyframes moveLeftRight {
  0% {
    transform: translateX(-100%); /* Start off-screen to the left */
  }
  50% {
    transform: translateX(0); /* Move to the center */
  }
  100% {
    transform: translateX(100%); /* Move off-screen to the right */
  }
}

.animate-move-left-right {
  animation: moveLeftRight 20s linear infinite; /* Apply animation */
}

.layout5 {
  position: relative;
  overflow: hidden;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.grid {
  display: grid;
  gap: 20px;
}

.grid-cols-1 {
  grid-template-columns: 1fr;
}

.md\:grid-cols-2 {
  grid-template-columns: 1fr 1fr;
}

@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }

  .grid {
    gap: 15px;
  }
}
</style>
