<template>
  <div class="flex flex-col items-center justify-center h-screen bg-gray-100">
    <img
      :src="currentImage"
      alt="Random image"
      class="w-96 h-64 object-cover rounded-lg shadow-md mb-4"
    />
    <button
      @click="changeImage"
      class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors"
    >
      Siguiente
    </button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const currentImage = ref("https://picsum.photos/400/300");

    const changeImage = () => {
      // Add a timestamp to force a new image
      currentImage.value = `https://picsum.photos/400/300?t=${Date.now()}`;
    };

    return {
      currentImage,
      changeImage,
    };
  },
};
</script>
