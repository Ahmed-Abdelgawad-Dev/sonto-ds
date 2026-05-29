<script setup>
import { onMounted } from "vue";

const services = [
  {
    icon: "💻",
    title: "Web Development",
    description:
      "We build modern, responsive web applications using cutting-edge frameworks and technologies. From simple landing pages to complex enterprise platforms, our solutions are fast, scalable, and user-friendly.",
    features: [
      "Custom Web Applications",
      "E-commerce Solutions",
      "CMS Development",
      "Progressive Web Apps",
      "API Development",
      "Performance Optimization",
    ],
    gradient: "from-primary to-purple-500",
    // price: "Custom Pricing",
  },
  {
    icon: "📱",
    title: "Mobile Development",
    description:
      "Native and cross-platform mobile applications designed to deliver exceptional user experiences. We leverage React Native, Flutter, and native technologies to build apps your users will love.",
    features: [
      "iOS & Android Apps",
      "Cross-Platform Solutions",
      "App UI/UX Design",
      "App Store Optimization",
      "Push Notifications",
      "Real-Time Features",
    ],
    gradient: "from-secondary to-cyan-500",
    // price: "Custom Pricing",
  },
  {
    icon: "📊",
    title: "Digital Strategy",
    description:
      "Data-driven strategies that help businesses navigate digital transformation. We analyze, plan, and execute comprehensive digital roadmaps for sustainable growth.",
    features: [
      "Digital Transformation",
      "Market Analysis",
      "Technology Consulting",
      "Growth Strategy",
      "Brand Strategy",
      "Analytics & Insights",
    ],
    gradient: "from-rose-500 to-red-500",
    // price: "Custom Pricing",
  },
  {
    icon: "☁️",
    title: "Cloud Solutions",
    description:
      "Scalable cloud infrastructure and DevOps solutions that ensure your applications run smoothly, securely, and efficiently. We specialize in AWS, Azure, and Google Cloud.",
    features: [
      "Cloud Migration",
      "Infrastructure as Code",
      "CI/CD Pipelines",
      "Microservices Architecture",
      "Monitoring & Logging",
      "Security & Compliance",
    ],
    gradient: "from-amber-500 to-orange-500",
    // price: "Custom Pricing",
  },
  {
    icon: "🎨",
    title: "UI/UX Design",
    description:
      "User-centered design that combines aesthetics with functionality. We create intuitive, accessible, and beautiful interfaces that engage users and drive conversions.",
    features: [
      "User Research",
      "Wireframing",
      "Prototyping",
      "Visual Design",
      "Design Systems",
      "Usability Testing",
    ],
    gradient: "from-emerald-500 to-teal-500",
    // price: "Custom Pricing",
  },
  {
    icon: "🤖",
    title: "Email Hosting & Deployment",
    description:
      "Our email hosting and deployment services ensure your emails reach your audience efficiently and securely. We offer a range of solutions to suit your needs.",
    features: [
      "Email Hosting",
      "Email Deployment",
      "Email Security",
      "Email Forwarding",
      "Email Analytics",
      "Email Automation",
    ],
    gradient: "from-purple-500 to-pink-500",
    // price: "Custom Pricing",
  },
];

const processSteps = [
  {
    step: "01",
    title: "Discovery",
    description: "We analyze your needs, goals, and vision",
  },
  {
    step: "02",
    title: "Strategy",
    description: "We create a comprehensive project roadmap",
  },
  {
    step: "03",
    title: "Design",
    description: "We craft beautiful, user-centered designs",
  },
  {
    step: "04",
    title: "Development",
    description: "We build with cutting-edge technology",
  },
  {
    step: "05",
    title: "Launch",
    description: "We deploy and ensure everything runs smoothly",
  },
  {
    step: "06",
    title: "Support",
    description: "We provide ongoing maintenance and support",
  },
];

const observeElements = () => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.setAttribute("data-animated", "true");
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
          Our
          <span class="gradient-text">Services</span>
        </h1>
        <p class="max-w-2xl mx-auto text-gray-400 text-lg">
          Comprehensive digital solutions tailored to your business needs. We
          deliver excellence across every service we provide.
        </p>
      </div>
    </section>

    <!-- Services Grid -->
    <section class="py-20 bg-darker">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="space-y-8">
          <div
            v-for="(service, index) in services"
            :key="index"
            class="group relative p-8 lg:p-10 rounded-2xl glass border border-white/5 hover:border-primary/20 transition-all duration-500 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <div class="flex flex-col lg:flex-row gap-8 items-start">
              <!-- Icon -->
              <div
                class="text-5xl lg:text-6xl flex-shrink-0 w-20 h-20 flex items-center justify-center rounded-2xl glass group-hover:scale-110 transition-transform duration-300"
              >
                {{ service.icon }}
              </div>

              <!-- Content -->
              <div class="flex-1">
                <div
                  class="w-16 h-1 rounded-full bg-gradient-to-r mb-4"
                  :class="service.gradient"
                ></div>
                <h3 class="text-2xl font-bold text-white mb-3">
                  {{ service.title }}
                </h3>
                <p class="text-gray-400 leading-relaxed mb-6">
                  {{ service.description }}
                </p>

                <!-- Features -->
                <div
                  class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3"
                >
                  <div
                    v-for="(feature, idx) in service.features"
                    :key="idx"
                    class="flex items-center gap-2 text-sm text-gray-300"
                  >
                    <svg
                      class="w-4 h-4"
                      :class="
                        'text-' +
                        service.gradient.split(' ')[0].replace('from-', '')
                      "
                      fill="currentColor"
                      viewBox="0 0 20 20"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                    {{ feature }}
                  </div>
                </div>
              </div>

              <!-- Price -->
              <div
                class="flex-shrink-0 text-right lg:border-l lg:border-white/5 lg:pl-8"
              >
                <p class="text-sm text-gray-500 mb-1">
                  {{ service.price }}
                </p>
                <router-link
                  to="/contact"
                  class="inline-flex items-center gap-2 text-sm font-medium text-primary hover:text-primary-dark transition-colors"
                >
                  Get Quote
                  <svg
                    class="w-4 h-4"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M17 8l4 4m0 0l-4 4m4-4H3"
                    />
                  </svg>
                </router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Process Section -->
    <section class="py-20 bg-dark">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
            Our
            <span class="gradient-text-purple">Process</span>
          </h2>
          <p class="text-gray-400 text-lg max-w-2xl mx-auto">
            A proven methodology that ensures successful project delivery from
            concept to launch.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            v-for="(step, index) in processSteps"
            :key="index"
            class="relative p-8 rounded-2xl glass border border-white/5 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <div
              class="text-4xl font-extrabold bg-gradient-to-br from-primary to-secondary bg-clip-text text-transparent mb-4"
            >
              {{ step.step }}
            </div>
            <h3 class="text-xl font-semibold text-white mb-2">
              {{ step.title }}
            </h3>
            <p class="text-gray-400 text-sm">{{ step.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section class="py-20 bg-dark">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-6">
          Not Sure Which Service You Need?
        </h2>
        <p class="text-gray-400 text-lg mb-8 max-w-2xl mx-auto">
          Schedule a free consultation with our experts. We'll help you find the
          perfect solution for your business.
        </p>
        <router-link
          to="/contact"
          class="inline-flex items-center gap-2 px-8 py-4 rounded-xl text-lg font-semibold text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300 btn-border-glow"
        >
          Book a Free Consultation
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
              d="M17 8l4 4m0 0l-4 4m4-4H3"
            />
          </svg>
        </router-link>
      </div>
    </section>
  </div>
</template>
