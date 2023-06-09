<script lang="ts" setup>
// Imports
import { type PropType, watch } from 'vue'
import { XMarkIcon } from '@heroicons/vue/24/outline'
import type { TailwindColor, DialogPosition } from './types'

// Props
const props = defineProps({
  modelValue: { type: Boolean as PropType<boolean>, default: false },
  position: { type: String as PropType<DialogPosition>, default: 'bottom' },
  color: { type: String as PropType<TailwindColor>, default: 'black' },
  timeout: { type: [Number,String] as PropType<number|string>, default: 5000 },
})

// Emits
const emits = defineEmits(['update:modelValue'])


/** UPDATE MODEL VALUE */
const updateModelValue = (value: Boolean) => {
  emits('update:modelValue', value)
}


/** CLOSE TOAST ON TIMEOUT */
let timeoutId: ReturnType<typeof setTimeout>;
watch(() => props.modelValue, () => {
    if (props.modelValue) {
      clearTimeout(timeoutId)
      timeoutId = setTimeout(() => updateModelValue(false), +props.timeout)
    }
  }
);
</script>

<template>
  <div
    v-if="modelValue"
    class="fixed bottom-0 left-1/2 mb-3 -translate-x-1/2 z-50 rounded py-2 px-3 font-semibold text-white flex items-center gap-5 shadow"
    :class="[
      `bg-${color}`
    ]"
  >
    <slot></slot>
    <button
      type="button"
      class="rounded-full outline-none p-px flex items-center justify-center text-white hover:bg-white/20"
      @click="updateModelValue(false)"
    >
      <XMarkIcon class="h-4 w-4" />
    </button>
  </div>
</template>

<style scoped></style>