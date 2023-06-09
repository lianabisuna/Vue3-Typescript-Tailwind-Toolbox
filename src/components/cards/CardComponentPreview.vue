<script lang="ts" setup>
// Imports
import { SunIcon, MoonIcon } from '@heroicons/vue/24/outline'
import { useDark } from '@vueuse/core'
import { type PropType, watch } from 'vue'

// Props
defineProps({
  dark: { type: Boolean as PropType<boolean>, default: false },
  title: { type: String as PropType<string>, default: '' },
})

// Emits
const emits = defineEmits(['update:dark'])


/** TOGGLE DARK MODE */

const updateDark = (event: Event) => {
  const target = event.target as HTMLInputElement
  if (target.value !== undefined) {
    emits('update:dark', target.checked)
  }
}

const isDark = useDark()

watch(isDark, () => emits('update:dark', isDark.value), { immediate: true });
</script>

<template>
  <div
    class="flex flex-col border dark:bg-gray-700 dark:border-gray-500 bg-gray-100 border-gray-300 max-w-full"
    v-bind="$attrs"
  >
    <!-- Header -->
    <div class="flex items-center justify-between border-b flex-shrink p-3 dark:border-gray-500 border-gray-300">
      <p class="text-gray-800 dark:text-white">{{ title }}</p>
      <!-- Light/Dark -->
      <label class="relative cursor-pointer">
        <input
          type="checkbox"
          :checked="dark"
          @input="updateDark"
          class="sr-only peer"
        >
        <div
          class="aspect-square p-2 bg-white active:shadow active:bg-transparent/10 hover:bg-gray-100 dark:hover:bg-gray-700 rounded border border-gray-400 dark:border-gray-500 peer dark:bg-gray-800 flex items-center justify-center dark:text-gray-100"
        >
          <SunIcon v-if="dark" class="h-5 w-5" />
          <MoonIcon v-else class="h-5 w-5" />
        </div>
      </label>
    </div>
    <!-- Body -->
    <div
      class="flex flex-1 items-center justify-center p-3"
      :class="[
        dark ? 'bg-gray-900' : 'bg-white'
      ]"
    >
      <slot></slot>
    </div>
  </div>
</template>

<style scoped></style>