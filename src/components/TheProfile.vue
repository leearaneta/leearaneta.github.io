<script setup lang="ts">
import profile from '@/assets/profile.jpeg'
</script>

<template>
  <div class="flex w-full">
    <img class="max-md:hidden" :src="profile"/>
    <div class="container flex gap-2 flex-wrap shrink-0">
      <div class="flex flex-col">
        <div class="flex gap-2 items-center">
          <span class="mb-1"> Lee Araneta </span>
          <a
            class="md:hidden"
            href="mailto:lee.araneta@gmail.com"
            target="_blank"
          >
            <svg class="w-6 h-6 mt-px" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M21.75 6.75v10.5a2.25 2.25 0 0 1-2.25 2.25h-15a2.25 2.25 0 0 1-2.25-2.25V6.75m19.5 0A2.25 2.25 0 0 0 19.5 4.5h-15a2.25 2.25 0 0 0-2.25 2.25m19.5 0v.243a2.25 2.25 0 0 1-1.07 1.916l-7.5 4.615a2.25 2.25 0 0 1-2.36 0L3.32 8.91a2.25 2.25 0 0 1-1.07-1.916V6.75" />
            </svg>
          </a>
          <a
            class="md:hidden"
            href="https://github.com/leearaneta"
            target="_blank"
          >
            <svg class="w-6 h-6 mt-px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 98 96">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M48.854 0C21.839 0 0 22 0 49.217c0 21.756 13.993 40.172 33.405 46.69 2.427.49 3.316-1.059 3.316-2.362 0-1.141-.08-5.052-.08-9.127-13.59 2.934-16.42-5.867-16.42-5.867-2.184-5.704-5.42-7.17-5.42-7.17-4.448-3.015.324-3.015.324-3.015 4.934.326 7.523 5.052 7.523 5.052 4.367 7.496 11.404 5.378 14.235 4.074.404-3.178 1.699-5.378 3.074-6.6-10.839-1.141-22.243-5.378-22.243-24.283 0-5.378 1.94-9.778 5.014-13.2-.485-1.222-2.184-6.275.486-13.038 0 0 4.125-1.304 13.426 5.052a46.97 46.97 0 0 1 12.214-1.63c4.125 0 8.33.571 12.213 1.63 9.302-6.356 13.427-5.052 13.427-5.052 2.67 6.763.97 11.816.485 13.038 3.155 3.422 5.015 7.822 5.015 13.2 0 18.905-11.404 23.06-22.324 24.283 1.78 1.548 3.316 4.481 3.316 9.126 0 6.6-.08 11.897-.08 13.526 0 1.304.89 2.853 3.316 2.364 19.412-6.52 33.405-24.935 33.405-46.691C97.707 22 75.788 0 48.854 0z" fill="#fff"/>
            </svg>
          </a>
        </div>
        <span class="max-md:hidden"> Fullstack Developer </span>
      </div>
      <div class="flex flex-col gap-1 max-md:hidden">
        <div class="flex gap-2">
          <svg class="w-6 h-6 mt-px" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M21.75 6.75v10.5a2.25 2.25 0 0 1-2.25 2.25h-15a2.25 2.25 0 0 1-2.25-2.25V6.75m19.5 0A2.25 2.25 0 0 0 19.5 4.5h-15a2.25 2.25 0 0 0-2.25 2.25m19.5 0v.243a2.25 2.25 0 0 1-1.07 1.916l-7.5 4.615a2.25 2.25 0 0 1-2.36 0L3.32 8.91a2.25 2.25 0 0 1-1.07-1.916V6.75" />
          </svg>
          <a
            class="hp"
            href="mailto:lee.araneta@gmail.com"
            target="_blank"
          >
            lee.araneta@gmail.com
          </a>
        </div>
        <div class="flex gap-2">
          <svg class="w-6 h-6 mt-px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 98 96">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M48.854 0C21.839 0 0 22 0 49.217c0 21.756 13.993 40.172 33.405 46.69 2.427.49 3.316-1.059 3.316-2.362 0-1.141-.08-5.052-.08-9.127-13.59 2.934-16.42-5.867-16.42-5.867-2.184-5.704-5.42-7.17-5.42-7.17-4.448-3.015.324-3.015.324-3.015 4.934.326 7.523 5.052 7.523 5.052 4.367 7.496 11.404 5.378 14.235 4.074.404-3.178 1.699-5.378 3.074-6.6-10.839-1.141-22.243-5.378-22.243-24.283 0-5.378 1.94-9.778 5.014-13.2-.485-1.222-2.184-6.275.486-13.038 0 0 4.125-1.304 13.426 5.052a46.97 46.97 0 0 1 12.214-1.63c4.125 0 8.33.571 12.213 1.63 9.302-6.356 13.427-5.052 13.427-5.052 2.67 6.763.97 11.816.485 13.038 3.155 3.422 5.015 7.822 5.015 13.2 0 18.905-11.404 23.06-22.324 24.283 1.78 1.548 3.316 4.481 3.316 9.126 0 6.6-.08 11.897-.08 13.526 0 1.304.89 2.853 3.316 2.364 19.412-6.52 33.405-24.935 33.405-46.691C97.707 22 75.788 0 48.854 0z" fill="#fff"/>
          </svg>
          <a
            class="mp"
            href="https://github.com/leearaneta"
            target="_blank"
          >
            github.com/leearaneta
          </a>
        </div>
      </div>
      <div class="md:hidden">
        <slot />
      </div>
    </div>
  </div>
</template>

<style scoped>
@media screen and (min-width: 768px) {
  .container {
    justify-content: space-between;
    flex-direction: column;
    margin-left: 2rem;
  }
}

@media screen and (max-width: 768px) {
  .container {
    padding-top: 1rem;
    padding-bottom: 1rem;
    padding-left: 2rem;
    padding-right: 2rem;
  }
}

.hp, .mp {
  padding-bottom: 2px;
  border: 4px solid;
  border-image-slice: 1;
  border-width: 2px;
  border-left: 0;
  border-right: 0;
  border-top: 0;
}

.hp {
  border-image-source: linear-gradient(to right, rgb(20, 102, 255) 60%,rgb(190, 213, 255));
}

.mp {
  border-image-source: linear-gradient(to right,rgb(20, 226, 123) 60%,rgb(217, 226, 222));
}
</style>