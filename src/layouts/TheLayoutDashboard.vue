<script lang="ts" setup>
// Imports
import { ref, provide, type Ref } from 'vue'
import Header from '@/components/TheHeader.vue'
import Sidebar from '../components/TheSidebar.vue'

// Types
export interface SidebarProvider {
  isSidebarOpen: Ref<boolean>
  toggleSidebar: CallableFunction
}


/** TOGGLE SIDEBAR */

// Data
const isSidebarOpen = ref(false)

// Function
const toggleSidebar = (value: boolean|undefined = undefined) => {
  isSidebarOpen.value = value !== undefined ? value : !isSidebarOpen.value
}

// Provide
provide('sidebar', { isSidebarOpen, toggleSidebar })
</script>

<template>
  <div class="relative flex flex-col h-screen overflow-hidden bg-white select-none dark:bg-gray-800">
    <!-- Content -->
    <main class="relative flex flex-1 max-w-screen">
      <Sidebar />
      <div class="h-full flex-1">
        <div class="flex flex-col h-full w-full">
          <Header />
          <div class="relative flex-1 overflow-hidden">
            <div class="absolute inset-0 h-full overflow-y-auto scrollbar">
              <slot></slot>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped></style>