<script setup lang="ts">

import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import { onMounted } from 'vue';

interface Image {
  src: string
}

interface Props {
  images: Image[]
}

const props = defineProps<Props>()

onMounted(() => {
  console.log(props.images)
})

</script>

<template>
  <div>
    <swiper>
      <swiper-slide v-for="img in props.images">
        <a href="#" target="_blank">
          <img :src="img.src" alt="Galerie Magali Martin" />
        </a>
      </swiper-slide>
    </swiper>

  </div>
</template>