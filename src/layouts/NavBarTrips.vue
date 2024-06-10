// eslint-disable-next-line vue/multi-word-component-names
<script setup lang="ts">
import { FwbNavbar, FwbNavbarCollapse, FwbNavbarLink, FwbNavbarLogo } from 'flowbite-vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const handleRoute = () => {
  router.push('/');
}
</script>

<template>
  <fwb-navbar class="fixed navbar">
    <template #logo>
      <fwb-navbar-logo class="text-gray-900" alt="Flowbite logo" image-url="./assets/images/trips-logo.png" link="#">
        Trips
      </fwb-navbar-logo>
    </template>
    <template #default="{ isShowMenu }">
      <fwb-navbar-collapse :is-show-menu="isShowMenu">
        <div
          class="px-6 gap-2 flex items-center justify-between py-3"
          @click="handleRoute"
          style="margin-top: -8px; background: #f44336; border-radius: 8px; cursor: pointer"
        >
          <span class="text-white">Find Trip </span>
          <svg
            class="w-6 h-6 text-white dark:text-white"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            viewBox="0 0 24 24"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M19 12H5m14 0-4 4m4-4-4-4"
            />
          </svg>
        </div>
      </fwb-navbar-collapse>
    </template>
  </fwb-navbar>
</template>
