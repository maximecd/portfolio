
<script setup lang="ts">
interface Image {
  src: string;
  key: string;
}

interface Props {
  images: Image[];
  alt: string;
  url: string;
}


const props = defineProps<Props>();
</script>


<template>
  <div class="scroll-container">
    <ul class="item-container">
      <li class="item" v-for="img in images">
        <a :href="url" target="_blank">
          <img :style="{ viewTransitionName: img.key }" :src="img.src" :alt="alt" />
        </a>
      </li>
    </ul>
  </div>
</template>

<style scoped lang="scss">
.scroll-container {
  overflow: scroll;
  scroll-snap-type: x mandatory;



}

.item {
  width: 100vw;
  scroll-snap-align: start;
}

.item-container {
  width: fit-content;
  display: grid;
  grid-auto-flow: column;
  gap: 3.6rem;
  padding: 0;
  list-style-type: none;


  &::-webkit-scrollbar {
    display: none;
  }

  scrollbar-width: none;


  @media (min-width: 768px) {
    flex-direction: row;
    max-width: 1500px;
    margin-inline: auto;
    width: 100%;
    justify-content: space-between;
    transform: translateY(20%);
    gap: 3.6rem;
    padding-inline: 2.4rem;
  }

}

img {
  box-shadow: 0px 2px 10px 1px rgba(255, 255, 255, 0.1);

  @media (min-width: 768px) {
    transition: all 1s cubic-bezier(0.165, 0.84, 0.44, 1);

    &:hover {
      transform: translateY(-10%);
    }
  }
}
</style>
