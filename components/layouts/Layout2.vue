
<template>
  <div class="layout2">
    <ClientOnly>
      <div class="relative max-h-screen overflow-hidden z-1">
        <!-- Lazy loading applied to NuxtImg -->
        <div class="relative max-h-screen overflow-hidden z-1">
        <!-- Container for background image -->
        <div class="background-image-container">
          <NuxtImg
          v-if="data.thumbnail"
          :src="data.thumbnail"
          class="w-screen opacity-80 bg-cover"
          :alt="`Thumbnail for ${data.title}`"
          format="webp"
          loading="lazy"
          @load="imageLoaded = true"
          :class="{ loaded: imageLoaded }"
          />
        </div>

          <!-- Loading spinner -->
          <div v-if="!imageLoaded" class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-75">
            <div class="spinner"></div>
          </div>

          <!-- Overlay content -->
          <div
            class="absolute inset-0 flex flex-col items-center justify-center bg-black bg-opacity-10 animate-fade animate-once animate-delay-[500ms]"
            v-if="imageLoaded"
          >
            <div class="container p-4">
              <!-- Content goes here -->
            </div>
          </div>
        </div>

        
        <!-- Loading spinner -->
        <div v-if="!imageLoaded" class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-75">
          <div class="spinner"></div>
        </div>

        <!-- Overlay content -->
        <div
          class="absolute inset-0 flex flex-col items-center justify-center bg-black bg-opacity-50 animate-fade animate-once animate-delay-[500ms]"
          v-if="imageLoaded"
        >
          <div class="container p-4">
            <div>
              <h1 class="text-amber-400 text-4xl md:text-6xl lg:text-8xl font-bold animate__animated animate__fadeInRight">{{ data.title }}</h1>
              <h1 v-if="data.subtitle" class="text-white opacity-80 pt-3 text-xl md:text-2xl lg:text-3xl font-bold pb-10 animate__animated animate__fadeIn animate__delay-1s">{{ data.subtitle }}</h1>
            </div>
            
            <div>
              <p v-if="data.author" class="text-white opacity-80 text-xs font-bold animate__animated animate__fadeIn animate__delay-2s">{{ data.author }}</p>
              <p class="text-white text-xs opacity-50 hover:opacity-100 animate__animated animate__fadeIn animate__delay-2.5s">Last update: {{ formatDate(data.date) }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Floating Drawer -->
      <div class="absolute top-0 right-0 z-10 pr-5">
        <Drawer />
      </div>

      <!-- Main Content -->
      <div class="container mx-auto px-4 py-8" v-if="imageLoaded">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <!-- First column -->
          <div>
            <h1 class="text-3xl md:text-4xl lg:text-5xl font-bold animate__animated animate__fadeIn">{{ data.title }}</h1>
            <h2 v-if="data.subtitle" class="opacity-80 text-xl md:text-2xl lg:text-3xl font-semibold pb-6 animate__animated animate__fadeIn animate__delay-1s">
              {{ data.subtitle }}
            </h2>
            <p class="text-lg md:text-xl lg:text-lg font-normal pb-4 animate__animated animate__fadeIn animate__delay-1.5s">{{ data.description }}</p>
            <div v-if="data.imagegallery?.showgallery" class="mt-4">
              <ImageGallery />
            </div>
          </div>

          <!-- Second column -->
          <div class="relative bg-gray-100 p-6 rounded-xl shadow-lg hover:scale-105 transition-transform duration-300 animate__animated animate__fadeIn animate__delay-2s">
            <ContentRenderer :value="data" />
          </div>
        </div>

        <!-- Related Pages -->
        <div v-if="data.related_page" class="mt-10">
          <RelatedPages :relatedPages="data.related_page" />
        </div>

        <!-- Tags and Last Update -->
        <div class="mt-10 text-xs leading-5">
          <hr class="my-4" />
          <p class="opacity-50 hover:opacity-100 pb-2 animate__animated animate__fadeIn animate__delay-3s">Last update: {{ formatDate(data.date) }}</p>
          <ul v-if="data.tags" class="tags flex flex-wrap gap-2">
            <li
              v-for="(tag, index) in data.tags"
              :key="index"
              class="text-xs opacity-50 hover:opacity-100 animate__animated animate__fadeIn animate__delay-3.5s"
            >
              <NuxtLink :to="`/tags/${tag}`" class="text-blue-500 underline">
                {{ tag }}
              </NuxtLink>
            </li>
          </ul>
        </div>
      </div>

      <!-- Share Buttons -->
      <div class="mt-10">
        <ShareButtons />
      </div>

      <!-- SEO Metadata -->
      <Title>{{ data.title }}</Title>
      <Meta name="description" :content="data.description" />
      <Meta name="tags" :content="data.tags.join(', ') || ''" />
      <Meta name="keywords" :content="data.tags.join(', ') || ''" />
      <Meta property="og:title" :content="data.title" />
      <Meta property="og:description" :content="data.description" />
      <Meta property="og:image" :content="data.thumbnail" />
      <Meta property="og:url" :content="data.url" />
      <Meta property="og:type" content="article" />
    </ClientOnly>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imageLoaded: false,
    };
  },
  props: {
    data: {
      type: Object,
      required: true,
    },
    formatDate: {
      type: Function,
      required: true,
    },
  },
  methods: {
    onImageLoad() {
      this.imageLoaded = true;
    },
  },
};
</script>

<style scoped>
.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #fff;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Fade Right Animation */
@keyframes fadeInRight {
  0% {
    opacity: 0;
    transform: translateX(30px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

.animate__fadeInRight {
  animation: fadeInRight 0.8s ease-out forwards;
}

/* Fade In Animation for other elements */
@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate__fadeIn {
  animation: fadeIn 0.5s ease-out forwards;
}
</style>


