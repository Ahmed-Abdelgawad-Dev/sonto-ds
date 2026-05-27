<script setup>
import { ref, watch, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";

const router = useRouter();
const route = useRoute();
const isMenuOpen = ref(false);
const isScrolled = ref(false);
const activeLink = ref("Home");

const navLinks = [
  { name: "Home", path: "/" },
  { name: "Services", path: "/services" },
  { name: "About", path: "/about" },
  { name: "Contact", path: "/contact" },
];

const handleScroll = () => {
  isScrolled.value = window.scrollY > 20;
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

const closeMenu = () => {
  isMenuOpen.value = false;
};

const navigateTo = (path) => {
  router.push(path);
  closeMenu();
};

watch(
  () => route.path,
  (path) => {
    const link = navLinks.find((l) => l.path === path);
    if (link) activeLink.value = link.name;
  },
);

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
  const link = navLinks.find((l) => l.path === route.path);
  if (link) activeLink.value = link.name;
});
</script>

<template>
  <nav
    :class="[
      'fixed top-0 left-0 right-0 z-50 transition-all duration-500',
      isScrolled ? 'glass-dark shadow-lg shadow-primary/5' : 'bg-transparent',
    ]"
  >
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-20">
        <!-- Logo -->
        <router-link
          to="/"
          class="flex items-center space-x-2 group"
          @click="closeMenu"
        >
          <div
            class="relative w-10 h-10 flex items-center justify-center rounded-xl gradient-primary shadow-lg shadow-primary/30 group-hover:shadow-primary/50 transition-all duration-300"
          >
            <span class="text-white font-extrabold text-lg">S</span>
          </div>
          <span
            class="text-2xl font-extrabold bg-gradient-to-r from-white to-gray-300 bg-clip-text text-transparent"
          >
            SONTO
          </span>
        </router-link>

        <!-- Desktop Navigation -->
        <div class="hidden lg:flex items-center space-x-1">
          <router-link
            v-for="link in navLinks"
            :key="link.name"
            :to="link.path"
            class="relative px-5 py-2.5 text-sm font-medium transition-all duration-300 rounded-lg nav-link-glow"
            :class="[
              activeLink === link.name
                ? 'text-white active-link'
                : 'text-gray-400 hover:text-white hover:bg-white/5',
            ]"
          >
            {{ link.name }}
            <span
              v-if="activeLink === link.name"
              class="absolute inset-0 bg-primary/10 rounded-lg border border-primary/20"
            ></span>
          </router-link>
        </div>

        <!-- CTA Button Desktop -->
        <div class="hidden lg:flex items-center space-x-4">
          <router-link
            to="/contact"
            class="relative px-6 py-2.5 rounded-xl text-sm font-semibold text-white gradient-primary overflow-hidden group transition-all duration-300 hover:shadow-lg hover:shadow-primary/30 btn-border-glow"
          >
            <span class="relative z-10">Get Started</span>
            <div
              class="absolute inset-0 bg-white/10 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
            ></div>
          </router-link>
        </div>

        <!-- Mobile Menu Button -->
        <button
          @click="toggleMenu"
          class="lg:hidden relative w-10 h-10 flex items-center justify-center rounded-lg bg-white/5 hover:bg-white/10 transition-all duration-300"
          aria-label="Toggle menu"
        >
          <div class="w-5 h-4 relative flex flex-col justify-between">
            <span
              :class="[
                'block h-0.5 w-full bg-white rounded-full transition-all duration-300',
                isMenuOpen ? 'rotate-45 translate-y-[7px]' : '',
              ]"
            ></span>
            <span
              :class="[
                'block h-0.5 w-full bg-white rounded-full transition-all duration-300',
                isMenuOpen ? 'opacity-0' : '',
              ]"
            ></span>
            <span
              :class="[
                'block h-0.5 w-full bg-white rounded-full transition-all duration-300',
                isMenuOpen ? '-rotate-45 -translate-y-[7px]' : '',
              ]"
            ></span>
          </div>
        </button>
      </div>
    </div>

    <!-- Mobile Menu -->
    <Transition
      enter-active-class="transition-all duration-300 ease-out"
      enter-from-class="opacity-0 max-h-0"
      enter-to-class="opacity-100 max-h-[500px]"
      leave-active-class="transition-all duration-200 ease-in"
      leave-from-class="opacity-100 max-h-[500px]"
      leave-to-class="opacity-0 max-h-0"
    >
      <div
        v-if="isMenuOpen"
        class="lg:hidden border-t border-white/5 glass-dark"
      >
        <div class="px-4 py-4 space-y-1">
          <router-link
            v-for="link in navLinks"
            :key="link.name"
            :to="link.path"
            @click="closeMenu"
            class="block px-4 py-3 rounded-xl text-sm font-medium transition-all duration-200"
            :class="
              activeLink === link.name
                ? 'text-white bg-primary/10 border border-primary/20'
                : 'text-gray-400 hover:text-white hover:bg-white/5'
            "
          >
            {{ link.name }}
          </router-link>
          <router-link
            to="/contact"
            @click="closeMenu"
            class="block px-4 py-3 mt-3 rounded-xl text-sm font-semibold text-white text-center gradient-primary"
          >
            Get Started
          </router-link>
        </div>
      </div>
    </Transition>
  </nav>
</template>
