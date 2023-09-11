<script setup lang="ts">

import { ref } from 'vue';

const scrollContainerRef = ref<HTMLDivElement | null>(null)

function manageScroll(direction: 'left' | 'right') {

  if (!scrollContainerRef.value) return

  let scrollContainer: HTMLDivElement = scrollContainerRef.value

  const gap =
    parseInt(getComputedStyle(scrollContainer).getPropertyValue('--gap').slice(0, -3)) *
    parseFloat(getComputedStyle(document.documentElement).fontSize)

  let firstItem: HTMLElement = scrollContainer.querySelector('.project') as HTMLElement
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
  <div class="projects">
    <div class="scroll-container" ref="scrollContainerRef">
      <slot></slot>
    </div>
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
  </div>
</template>

<style scoped lang="scss">
nav {
  margin-top: 1.6rem;
  display: flex;
  justify-content: flex-end;
}

.arrows {
  display: flex;
  gap: 1.6rem;
}

.scroll-container {
  overflow: scroll;
  scroll-snap-type: x mandatory;
  scroll-padding: 2.4rem;

  &::-webkit-scrollbar {
    display: none;
  }

  scrollbar-width: none;
  --gap: 1.6rem;

  @media (min-width: 768px) {
    // for the scroll padding
    // i want the item at the center of page (900px wrap)
    scroll-padding: calc((100vw - 900px) / 2);
    --gap: 7.2rem;
  }

  user-select: none;
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
</style>
