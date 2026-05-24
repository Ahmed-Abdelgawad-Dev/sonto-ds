<script setup>
import Navbar from "@/components/Navbar.vue";
import Footer from "@/components/Footer.vue";
import { ref, onMounted } from "vue";

const isLoading = ref(true);

onMounted(() => {
  setTimeout(() => {
    isLoading.value = false;
  }, 500);
});
</script>

<template>
  <!-- Loading Screen -->
  <Transition name="fade">
    <div
      v-if="isLoading"
      class="fixed inset-0 z-[100] bg-dark flex items-center justify-center"
    >
      <div class="text-center">
        <div class="relative w-20 h-20 mx-auto mb-6">
          <div
            class="absolute inset-0 border-4 border-primary/20 rounded-full"
          ></div>
          <div
            class="absolute inset-0 border-4 border-transparent border-t-primary rounded-full animate-spin"
          ></div>
          <div
            class="absolute inset-2 bg-gradient-to-br from-primary to-secondary rounded-full animate-pulse"
          ></div>
        </div>
        <span class="text-2xl font-bold gradient-text">SONTO</span>
      </div>
    </div>
  </Transition>

  <!-- Main Content -->
  <div class="min-h-screen bg-dark overflow-hidden">
    <Navbar />
    <main>
      <router-view v-slot="{ Component, route }">
        <Transition name="page" mode="out-in">
          <component :is="Component" :key="route.path" />
        </Transition>
      </router-view>
    </main>
    <Footer />
  </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.page-enter-active {
  animation: fade-in 0.4s ease-out;
}

.page-leave-active {
  animation: fade-in 0.2s ease-in reverse;
}
</style>
