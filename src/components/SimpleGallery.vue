<template>
  <div class="h-screen w-full flex justify-center items-center bg-black">
    <div
      :id="galleryID"
      class="relative w-[320px] h-[320px] sm:w-[550px] sm:h-[550px] p-2 bg-white"
    >
      <span class="bg-white p-2 opacity-50 absolute"
        >{{ currentIndex + 1 }}/{{ imagesData.length }}</span
      >
      <!-- Prev -->
      <button type="button" @click.prevent="prevNext" class="left-btn">
        <IconLeft class="w-[30px]" />
      </button>
      <!-- Next -->
      <button type="button" @click.prevent="nextImage" class="right-btn">
        <IconRight class="w-[30px]" />
      </button>
      <a
        v-for="(image, index) in imagesData"
        :key="index"
        :href="image.largeURL"
        :data-pswp-width="image.width"
        :data-pswp-height="image.height"
        target="_blank"
        rel="noreferrer"
        :class="{ hidden: Number(index) !== currentIndex }"
      >
        <img :src="image.thumbnailURL" alt="" class="w-full h-full object-fill" />
      </a>
    </div>
  </div>
</template>

<script>
import PhotoSwipeLightbox from 'photoswipe/lightbox'
import 'photoswipe/style.css'
import { ref } from 'vue'
import IconRight from '@/components/icons/IconRight.vue'
import IconLeft from '@/components/icons/IconLeft.vue'

export default {
  name: 'SimpleGallery',
  components: { IconLeft, IconRight },
  props: {
    galleryID: String,
    images: Array
  },

  setup(props) {
    const currentIndex = ref(0)

    function nextImage() {
      if (props?.images.length) {
        if (currentIndex.value < props.images.length - 1) {
          currentIndex.value++
        }
      }
    }

    function prevNext() {
      if (props?.images.length) {
        if (currentIndex.value > 0) {
          currentIndex.value--
        }
      }
    }
    return {
      currentIndex,
      nextImage,
      prevNext,
      imagesData: props.images
    }
  },

  mounted() {
    if (!this.lightbox) {
      this.lightbox = new PhotoSwipeLightbox({
        gallery: '#' + this.$props.galleryID,
        children: 'a',
        pswpModule: () => import('photoswipe')
      })
      this.lightbox.init()
    }
  },
  unmounted() {
    if (this.lightbox) {
      this.lightbox.destroy()
      this.lightbox = null
    }
  },
  methods: {}
}
</script>
