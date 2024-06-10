<script setup lang="ts">
import { RouterView, useRoute } from 'vue-router'
import NavBar from './layouts/NavBar.vue'
import NavBarTrips from './layouts/NavBarTrips.vue'
import Footer from './layouts/Footer.vue'
import { ref, watch } from 'vue';

const route = useRoute()
const currentPath = ref(route.path);

watch(route, (newRoute) => {
  currentPath.value = newRoute.path;
});
</script>

<template>
  <NavBarTrips v-if="currentPath === '/trips'" />
  <NavBar v-else />
  <RouterView />
  <Footer />
</template>
