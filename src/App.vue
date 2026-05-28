<script setup>
import Navbar from "@/components/Navbar.vue";
import Footer from "@/components/Footer.vue";
import WhatsAppButton from "@/components/WhatsAppButton.vue";
</script>

<template>
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
    <WhatsAppButton />
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
