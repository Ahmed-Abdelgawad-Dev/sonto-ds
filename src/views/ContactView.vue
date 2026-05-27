<script setup>
import { ref, onMounted } from "vue";

const form = ref({
  name: "",
  email: "",
  phone: "",
  service: "",
  message: "",
});

const submitted = ref(false);

const services = [
  "Web Development",
  "Email Hosting",
  "Mobile Development",
  "Cloud Services",
  "Digital Strategy & Marketing",
  "Other",
];

const contactInfo = [
  {
    icon: "📧",
    label: "Email",
    value: "contact@sonto.dev",
    href: "mailto:contact@sonto.dev",
  },
  {
    icon: "📞",
    label: "Phone",
    value: "+20 1018200400",
    href: "tel:+20 1018200400",
  },
  {
    icon: "📍",
    label: "Location",
    value: "Cairo, Egypt",
    href: "#",
  },
  {
    icon: "🕐",
    label: "Hours",
    value: "Sun - Thu: 9AM - 6PM",
    href: "#",
  },
];

const handleSubmit = () => {
  // Form submission logic would go here
  submitted.value = true;
  setTimeout(() => {
    submitted.value = false;
    form.value = { name: "", email: "", phone: "", service: "", message: "" };
  }, 3000);
};

const observeElements = () => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("animate-fade-in");
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.1 },
  );

  document.querySelectorAll(".animate-on-scroll").forEach((el) => {
    observer.observe(el);
  });
};

onMounted(() => {
  observeElements();
});
</script>

<template>
  <div class="pt-32">
    <!-- Header -->
    <section class="relative py-20 overflow-hidden">
      <div class="absolute inset-0 gradient-hero"></div>
      <div class="absolute inset-0 bg-grid-pattern"></div>
      <div
        class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-primary/10 rounded-full blur-3xl"
      ></div>

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h1 class="text-5xl md:text-6xl font-bold text-white mb-6">
          Get in
          <span class="gradient-text">Touch</span>
        </h1>
        <p class="max-w-2xl mx-auto text-gray-400 text-lg">
          Have a project in mind? We'd love to hear from you. Let's discuss how
          we can help bring your ideas to life.
        </p>
      </div>
    </section>

    <!-- Contact Section -->
    <section class="py-20 bg-darker">
      <div
        class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 lg:grid-cols-2 gap-16"
      >
        <!-- Contact Form -->
        <div class="animate-on-scroll opacity-0">
          <div class="p-8 lg:p-10 rounded-2xl glass border border-white/5">
            <h2 class="text-2xl font-bold text-white mb-2">
              Send Us a Message
            </h2>
            <p class="text-gray-400 text-sm mb-8">
              Fill out the form below and we'll get back to you within 24 hours.
            </p>

            <form @submit.prevent="handleSubmit" class="space-y-6">
              <!-- Name -->
              <div>
                <label
                  for="name"
                  class="block text-sm font-medium text-gray-300 mb-2"
                >
                  Full Name
                </label>
                <input
                  id="name"
                  v-model="form.name"
                  type="text"
                  required
                  placeholder="Your Name"
                  class="w-full px-4 py-3 rounded-xl bg-white/5 border border-white/10 text-white placeholder-gray-500 focus:outline-none focus:border-primary/50 focus:ring-1 focus:ring-primary/30 transition-all duration-300"
                />
              </div>

              <!-- Email -->
              <div>
                <label
                  for="email"
                  class="block text-sm font-medium text-gray-300 mb-2"
                >
                  Email Address
                </label>
                <input
                  id="email"
                  v-model="form.email"
                  type="email"
                  required
                  placeholder="email@example.com"
                  class="w-full px-4 py-3 rounded-xl bg-white/5 border border-white/10 text-white placeholder-gray-500 focus:outline-none focus:border-primary/50 focus:ring-1 focus:ring-primary/30 transition-all duration-300"
                />
              </div>

              <!-- Phone -->
              <div>
                <label
                  for="phone"
                  class="block text-sm font-medium text-gray-300 mb-2"
                >
                  Phone Number
                </label>
                <input
                  id="phone"
                  v-model="form.phone"
                  type="tel"
                  placeholder="+20 1018200400"
                  class="w-full px-4 py-3 rounded-xl bg-white/5 border border-white/10 text-white placeholder-gray-500 focus:outline-none focus:border-primary/50 focus:ring-1 focus:ring-primary/30 transition-all duration-300"
                />
              </div>

              <!-- Service -->
              <div>
                <label
                  for="service"
                  class="block text-sm font-medium text-gray-300 mb-2"
                >
                  Service Interest
                </label>
                <select
                  id="service"
                  v-model="form.service"
                  class="w-full px-4 py-3 rounded-xl bg-white/5 border border-white/10 text-white focus:outline-none focus:border-primary/50 focus:ring-1 focus:ring-primary/30 transition-all duration-300"
                >
                  <option value="" disabled class="bg-dark">
                    Select a service
                  </option>
                  <option
                    v-for="service in services"
                    :key="service"
                    :value="service"
                    class="bg-dark"
                  >
                    {{ service }}
                  </option>
                </select>
              </div>

              <!-- Message -->
              <div>
                <label
                  for="message"
                  class="block text-sm font-medium text-gray-300 mb-2"
                >
                  Your Message
                </label>
                <textarea
                  id="message"
                  v-model="form.message"
                  required
                  rows="5"
                  placeholder="Tell us about your project..."
                  class="w-full px-4 py-3 rounded-xl bg-white/5 border border-white/10 text-white placeholder-gray-500 focus:outline-none focus:border-primary/50 focus:ring-1 focus:ring-primary/30 transition-all duration-300 resize-none"
                ></textarea>
              </div>

              <button
                type="submit"
                class="w-full py-4 rounded-xl text-lg font-semibold text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300 btn-border-glow"
              >
                Send Message
              </button>

              <!-- Success Message -->
              <Transition
                enter-active-class="transition-all duration-300 ease-out"
                enter-from-class="opacity-0 translate-y-2"
                enter-to-class="opacity-100 translate-y-0"
                leave-active-class="transition-all duration-200 ease-in"
                leave-from-class="opacity-100 translate-y-0"
                leave-to-class="opacity-0 translate-y-2"
              >
                <div
                  v-if="submitted"
                  class="p-4 rounded-xl bg-green-500/10 border border-green-500/20 text-green-400 text-sm text-center"
                >
                  ✅ Thank you! We'll get back to you soon.
                </div>
              </Transition>
            </form>
          </div>
        </div>

        <!-- Contact Info -->
        <div class="space-y-8 animate-on-scroll opacity-0">
          <div>
            <h2 class="text-2xl font-bold text-white mb-2">
              Contact Information
            </h2>
            <p class="text-gray-400 text-sm mb-8">
              Reach out to us through any of the channels below.
            </p>
          </div>

          <div class="space-y-4">
            <div
              v-for="(info, index) in contactInfo"
              :key="index"
              class="group p-6 rounded-xl glass border border-white/5 hover:border-primary/20 transition-all duration-300"
            >
              <a
                :href="info.href"
                class="flex items-center gap-4"
                :class="info.href === '#' ? 'cursor-default' : ''"
              >
                <div
                  class="text-2xl w-12 h-12 flex items-center justify-center rounded-xl bg-white/5 group-hover:scale-110 transition-transform duration-300"
                >
                  {{ info.icon }}
                </div>
                <div>
                  <p class="text-sm text-gray-500">{{ info.label }}</p>
                  <p class="text-white font-medium">{{ info.value }}</p>
                </div>
              </a>
            </div>
          </div>

          <!-- Map placeholder -->
          <div class="p-8 rounded-2xl glass border border-white/5 text-center">
            <div class="text-4xl mb-4">🌍</div>
            <h3 class="text-white font-semibold mb-2">Visit Our Office</h3>
            <p class="text-gray-400 text-sm">Giza, Cairo<br />Cairo, Egypt</p>
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ CTA -->
    <section class="py-20 bg-dark">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h2 class="text-4xl font-bold text-white mb-6">
          Prefer to
          <span class="gradient-text">Call Us</span>?
        </h2>
        <p class="text-gray-400 text-lg mb-8 max-w-xl mx-auto">
          Our team is available during business hours to answer any questions
          you may have.
        </p>
        <a
          href="tel:+201018200400"
          class="inline-flex items-center gap-2 px-8 py-4 rounded-xl text-lg font-semibold text-white glass border border-white/10 hover:border-primary/30 hover:bg-white/5 transition-all duration-300"
        >
          <svg
            class="w-5 h-5"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"
            />
          </svg>
          +201 018200400
        </a>
      </div>
    </section>
  </div>
</template>
