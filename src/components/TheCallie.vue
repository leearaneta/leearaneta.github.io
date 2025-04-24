<template>
  <div class="flex h-full justify-center">
    <div v-if="urls.length > 0" class="flex w-full" ref="container">
      <Transition :key="imageIndex" :name="transition" appear>
        <div
          class="flex flex-wrap justify-evenly cursor-pointer"
          @click="transitionForward()"
          @touchstart="touchstartX = $event.changedTouches[0].screenX"
          @touchend="onTouchEnd"
        >
          <img
            class="image"
            :class="containerType"
            :loading="imageLoadStrategy"
            :src="currentUrl"
            style="transform: translateZ(0); backface-visibility: hidden;"
            @load="handleImageLoad"
          />
        </div>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref, watch, computed } from 'vue';
import cursormove from '@/assets/cursormove.mp3'

const props = defineProps<{ urls: string[], chunkSize: number }>()
const imageIndex = ref(0)
const loadedImageCount = ref(props.chunkSize)
const transition = ref('forward')
const isImageLoading = ref(false)
const loadedImages = ref(new Set())

const currentUrl = computed(() => props.urls[imageIndex.value])
const imageLoadStrategy = computed(() => imageIndex.value < loadedImageCount.value ? 'eager' : 'lazy')

// Pre-load next few images
function preloadNextImages(currentIndex: number) {
  // Preload next 3 images
  const nextIndices = [...Array(props.chunkSize).keys()].map(offset => {
    const nextIndex = currentIndex + offset;
    return nextIndex >= props.urls.length ? nextIndex - props.urls.length : nextIndex;
  });
  
  nextIndices.forEach(index => {
    if (props.urls[index] && !loadedImages.value.has(props.urls[index])) {
      const img = new Image();
      img.onload = () => loadedImages.value.add(props.urls[index]);
      img.src = props.urls[index];
    }
  });
}

function handleImageLoad() {
  isImageLoading.value = false;
  loadedImages.value.add(currentUrl.value);
}

const audio = new Audio(cursormove)
function transitionForward() {
  // Prevent rapid clicking during transitions
  if (isImageLoading.value) return;
  
  transition.value = 'forward'
  isImageLoading.value = true;
  
  if (imageIndex.value === props.urls.length - 1) {
    imageIndex.value = 0
  } else {
    imageIndex.value += 1
  }
  
  // If already loaded, mark as not loading
  if (loadedImages.value.has(props.urls[imageIndex.value])) {
    isImageLoading.value = false;
  }
  
  audio.play()
}

function transitionBackward() {
  transition.value = 'backward'
  if (imageIndex.value === 0) {
    imageIndex.value = props.urls.length - 1
  } else {
    imageIndex.value -= 1
  }
  audio.play()
}

function handleKeyDown(e: KeyboardEvent) {
  e.preventDefault()
  if (e.key === 'ArrowRight' || e.key === 'Enter') {
    transitionForward()
  } else if (e.key === 'ArrowLeft') {
    transitionBackward()
  }
}

const touchstartX = ref<number>()
    
function onTouchEnd(e: TouchEvent) {
  const touchendX = e.changedTouches[0].screenX
  const thresholdPx = 25
  if (!touchstartX.value) {
    return
  }
  if ((touchendX + thresholdPx) < touchstartX.value) {
    transitionForward()
  } else if ((touchendX - thresholdPx) > touchstartX.value){
    transitionBackward()
  }
  touchstartX.value = undefined
}

const container = ref<HTMLElement>()
const containerType = ref('wide')

function debounce(callback: any, wait: number) {
  let timeoutId: any = null;
  return (...args: any[]) => {
    window.clearTimeout(timeoutId);
    timeoutId = window.setTimeout(() => {
      callback(...args);
    }, wait);
  };
}

function setContainerType() {
  if (container.value?.clientHeight && container.value?.clientWidth) {
    if (container.value?.clientHeight > container.value?.clientWidth) {
      containerType.value = 'tall'
    } else {
      containerType.value = 'wide'
    }
  }
}

const resizeObserver = ref(
  new ResizeObserver(debounce(setContainerType, 100))
)

watch(container, (newVal, oldVal) => {
  if (newVal) {
    if (!oldVal) {
      resizeObserver.value.observe(newVal)
    }
  } else {
    if (oldVal) {
      resizeObserver.value.unobserve(oldVal)
    }
  }
})

// Watch for index changes to preload
watch(imageIndex, (newIndex) => {
  preloadNextImages(newIndex);
});

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown, false);
  // Initial preload
  if (props.urls.length > 0) {
    preloadNextImages(imageIndex.value);
  }
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown, false)
})

</script>

<style lang="scss" scoped>
.image {
  object-fit: contain;
  &.wide {
    max-height: 100%;
  }
  &.tall {
    max-width: 100%;
  }
}

.forward-enter-active,
.backward-enter-active {
  transition: transform 0.15s cubic-bezier(0.2, 0, 0.2, 1);
  will-change: transform;
  transform: translateZ(0);
  backface-visibility: hidden;
}

.forward-enter-from {
  transform: translateX(500px);
}

.backward-enter-from {
  transform: translateX(-500px);
}
</style>
