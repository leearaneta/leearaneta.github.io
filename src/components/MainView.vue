<script setup lang="ts">
import { onMounted, ref, watch, onBeforeMount } from 'vue';
import cursormove from '@/assets/cursormove.mp3'
import VerticalSelection from './VerticalSelection.vue';
import TheProfile from './TheProfile.vue';
import ForWork from './ForWork.vue'
import AboutMe from './AboutMe.vue';
import ForFun from './ForFun.vue';
import TheCallie from './TheCallie.vue';

const width = ref(window.innerWidth)
const urls = ref<string[]>([])
const chunkSize = 10

// Audio context and buffer setup
const audioContext = ref<AudioContext | null>(null)
const audioBuffer = ref<AudioBuffer | null>(null)

// Function to play the sound using Web Audio API
function playCursorSound() {
  if (audioContext.value && audioBuffer.value) {
    const source = audioContext.value.createBufferSource()
    source.buffer = audioBuffer.value
    source.connect(audioContext.value.destination)
    source.start(0)
  }
}

// Initialize audio context and load buffer
async function initAudio() {
  try {
    // Create audio context with browser compatibility
    audioContext.value = new (window.AudioContext || (window as any).webkitAudioContext)()
    
    // Fetch the audio file
    const response = await fetch(cursormove)
    const arrayBuffer = await response.arrayBuffer()
    
    // Decode the audio data
    audioBuffer.value = await audioContext.value.decodeAudioData(arrayBuffer)
  } catch (error) {
    console.error('Failed to initialize audio:', error)
  }
}

function preloadImages(urls: string[], count: number) {
  urls.slice(0, count).forEach((url) => {
    const imageEl = new Image()
    imageEl.src = url
  })
}

onBeforeMount(() => {
  // Initialize audio on component mount
  initAudio()
})

onMounted(async () => {
  window.onresize = () => {
    width.value = window.innerWidth
  }
  const response = await fetch('https://cdn.jsdelivr.net/gh/leearaneta/callie@main/files.txt')
  const text = await response.text()
  urls.value = text.split('\n')
    .map(filename => `https://cdn.jsdelivr.net/gh/leearaneta/callie@main/${filename}`)
    .map(value => ({ value, sort: Math.random() }))
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value)
  
  // preload first chunk of images after URLs are fetched
  preloadImages(urls.value, chunkSize)
})

const selectedIndex = ref<number>(0)

watch(selectedIndex, () => {
  playCursorSound()
})

const options = ['About Me', 'For Work', 'For Fun', 'Callie']
</script>

<template>
  <div class="grid">
    <Transition name="left" appear>
      <div class="profile">
        <div class="h-full section profile-padding flex">
          <TheProfile>
            <VerticalSelection
              v-if="width <= 768"
              :options="options"
              :selectedIndex="selectedIndex"
              @select="selectedIndex = $event"
            />
          </TheProfile>
        </div>
      </div>
    </Transition>
    <Transition name="down" appear>
      <div class="menu max-md:hidden">
        <div class="h-full section py-4 px-2">
          <VerticalSelection
            v-if="width > 768"
            :options="options"
            :selectedIndex="selectedIndex"
            @select="selectedIndex = $event"
          />
        </div>
      </div>
    </Transition>
    <Transition name="left" appear>
      <div class="body">
        <div class="h-full section py-4 px-10 pr-28 max-md:px-6 flex flex-col">
          <div class="h-full overflow-scroll">
            <ForWork v-if="options[selectedIndex] === 'For Work'" />
            <AboutMe v-if="options[selectedIndex] === 'About Me'" />
            <ForFun v-if="options[selectedIndex] === 'For Fun'" />
            <TheCallie v-if="options[selectedIndex] === 'Callie'" :urls="urls" :chunkSize="chunkSize" :playCursorSound="playCursorSound" />
          </div>
        </div>
      </div>
    </Transition>
    <!-- <Transition name="right" appear>
      <div class="time max-md:hidden">
        <div class="h-full section p-4 flex flex-col justify-around">
          <div> inspired by the </div>
          <a
            href="https://i.ytimg.com/vi/VM-h84dIPJ4/maxresdefault.jpg"
            target="_blank"
          >
            final fantasy 7 menu
          </a>
        </div>
      </div>
    </Transition> -->
    <Transition name="right" appear>
      <div class="location max-md:hidden">
        <div class="h-full section p-4 flex items-center"> Brooklyn, NY </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.grid {
  height: 100%;
}

@media screen and (min-width: 1000px) {
  .grid {
    max-width: 1000px;
  }
}

@media screen and (min-width: 768px) {
  .grid {
    margin: auto;
    grid-template-columns: 1fr 36rem 5rem 12rem;
    grid-template-rows: 2.5rem 12rem 1fr 4rem 8rem 1.5rem 3rem;
  }

  .profile {
    grid-column: 1 / 4;
    grid-row: 2 / 3;
  }

  .profile-padding {
    padding-top: 1rem;
    padding-bottom: 1rem;
    padding-left: 2.5rem;
    padding-right: 5rem;
  }

  .body {
    grid-column: 1 / 4;
    grid-row: 3 / 7;
  }

  .menu {
    grid-column: 3 / 5;
    grid-row: 1 / 4;
  }

  .time {
    grid-column: 3 / 5;
    grid-row: 5 / 6;
  }

  .location {
    grid-column: 3 / 5;
    grid-row: 6 / 8;
  }
}

@media screen and (max-width: 768px) {
  .grid {
    grid-template-rows: 25vh 75vh;
    overflow: hidden;
  }

  .profile {
    grid-row: 1;
  }

  .scrollable {
    grid-row: 2;
  }

  .profile-padding {
    padding-top: 0.25rem;
    padding-bottom: 0.25rem;
  }

}

.menu, .time, .location {
  z-index: 1;
}

.left-enter-active,
.down-enter-active,
.right-enter-active,
.up-enter-active {
  transition: transform 0.4s ease;
}

.left-enter-from {
  transform: translateX(1000px)
}

.down-enter-from {
  transform: translateY(-1000px)
}

.right-enter-from {
  transform: translateX(-1000px)
}

.up-enter-from {
  transform: translateY(1000px)
}

</style>