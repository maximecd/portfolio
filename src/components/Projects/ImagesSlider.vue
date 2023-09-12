
<script setup lang="ts">

import { ref } from 'vue';


interface Image {
  src: string;
  key?: string;
}

interface Props {
  images: Image[];
  alt: string;
  url: string;
}


const props = defineProps<Props>();


const scrollContainerRef = ref<HTMLDivElement | null>(null)

function manageScroll(direction: 'left' | 'right') {

  if (!scrollContainerRef.value) return

  let scrollContainer: HTMLDivElement = scrollContainerRef.value

  const gap =
    parseInt(getComputedStyle(scrollContainer).getPropertyValue('--gap').slice(0, -3)) *
    parseFloat(getComputedStyle(document.documentElement).fontSize)

  let firstItem: HTMLElement = scrollContainer.querySelector('.item') as HTMLElement
  let itemWidth = firstItem.offsetWidth

  if (direction === 'left') {
    scrollContainer.scrollTo({
      left: scrollContainer.scrollLeft - itemWidth - gap,
      behavior: 'smooth'
    })
  } else if (direction === 'right') {
    scrollContainer.scrollTo({
      left: scrollContainer.scrollLeft + itemWidth + gap,
      behavior: 'smooth'
    })
  }
}

</script>


<template>
  <div>
    <nav class="wrapper">
      <div class="arrows">
        <button id="work-prev" aria-label="Previous Project" @click="manageScroll('left')">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
            class="lucide lucide-chevron-left">
            <path d="m15 18-6-6 6-6"></path>
          </svg>
        </button>
        <button id="work-next" aria-label="Next Project" @click="manageScroll('right')">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
            class="lucide lucide-chevron-right">
            <path d="m9 18 6-6-6-6"></path>
          </svg>
        </button>
      </div>
    </nav>
    <div class="scroll-container" ref="scrollContainerRef">
      <ul class="item-container">
        <li class="item" v-for="img in images">
          <a :href="url" target="_blank">
            <img :style="{ viewTransitionName: img.key }" :src="img.src" :alt="alt" />
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped lang="scss">
nav {
  margin-bottom: 1.6rem;
  display: flex;
  justify-content: flex-end;

  @media (min-width: 768px) {
    display: none;
  }
}

.arrows {
  display: flex;
  gap: 1.6rem;
}


button {
  display: grid;
  place-items: center;
  padding: 0;
  width: 4rem;
  height: 4rem;
  border: none;
  background-color: rgb(238, 238, 238);
  border-radius: 99px;
  color: var(--clr-text-light);
  cursor: pointer;

  @media (hover: hover) {
    &:hover {
      background-color: rgb(212, 212, 212);
      color: var(--clr-text);
    }
  }

  &:active {
    background-color: rgb(212, 212, 212);
    color: var(--clr-text);
  }
}

.scroll-container {
  --gap: 3.6rem;
  overflow: scroll;
  scroll-snap-type: x mandatory;

  &::-webkit-scrollbar {
    display: none;
  }

  scrollbar-width: none;

  @media (min-width: 768px) {
    overflow: hidden;
    scroll-snap-type: none;
  }

}

.item {
  width: 100vw;
  scroll-snap-align: start;

  @media (min-width: 768px) {
    width: auto;
    scroll-snap-align: none;
  }

}

.item-container {
  width: fit-content;
  display: grid;
  grid-auto-flow: column;
  gap: 3.6rem;
  padding: 0;
  list-style-type: none;

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
  box-shadow: var(--box-shadow);
  width: 100%;

  @media (min-width: 768px) {
    transition: all 1s cubic-bezier(0.165, 0.84, 0.44, 1);

    &:hover {
      transform: translateY(-10%);
    }
  }
}
</style>
