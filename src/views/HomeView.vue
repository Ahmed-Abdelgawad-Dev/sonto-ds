<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const animatedElements = ref([]);

const services = [
  {
    icon: "💻",
    title: "Web Development",
    description:
      "Custom websites and web applications built with cutting-edge technologies for optimal performance and user experience.",
    gradient: "from-primary to-purple-500",
  },
  {
    icon: "📱",
    title: "Mobile Apps",
    description:
      "Native and cross-platform mobile applications that deliver seamless experiences across all devices and platforms.",
    gradient: "from-secondary to-cyan-500",
  },
  {
    icon: "🤖",
    title: "AI Solutions",
    description:
      "Intelligent automation and machine learning solutions that transform your business processes and decision-making.",
    gradient: "from-purple-500 to-pink-500",
  },
  {
    icon: "☁️",
    title: "Cloud Services",
    description:
      "Scalable cloud infrastructure and DevOps solutions that ensure reliability, security, and cost efficiency.",
    gradient: "from-amber-500 to-orange-500",
  },
  {
    icon: "🎨",
    title: "UI/UX Design",
    description:
      "User-centered design that combines aesthetics with functionality to create memorable digital experiences.",
    gradient: "from-emerald-500 to-teal-500",
  },
  {
    icon: "📊",
    title: "Digital Strategy",
    description:
      "Data-driven digital transformation strategies that help businesses adapt, grow, and thrive in the digital age.",
    gradient: "from-rose-500 to-red-500",
  },
];

const stats = [
  { value: "150+", label: "Projects Delivered" },
  { value: "50+", label: "Happy Clients" },
  { value: "5+", label: "Years Experience" },
  { value: "15+", label: "Team Members" },
];

const testimonials = [
  {
    name: "Sarah Johnson",
    role: "CEO, TechVentures",
    content:
      "SONTO transformed our digital presence completely. Their team's expertise in web development and AI solutions is unmatched. We saw a 200% increase in engagement within months.",
    avatar: "SJ",
    gradient: "from-primary to-purple-500",
  },
  {
    name: "Michael Chen",
    role: "CTO, DataFlow Inc",
    content:
      "Working with SONTO was a game-changer for our business. Their cloud solutions helped us scale efficiently while reducing costs by 40%. Highly recommended!",
    avatar: "MC",
    gradient: "from-secondary to-cyan-500",
  },
  {
    name: "Emily Rodriguez",
    role: "Founder, CreativeLab",
    content:
      "The UI/UX design team at SONTO truly understands user experience. They created an intuitive interface that our customers love. Professional, creative, and reliable.",
    avatar: "ER",
    gradient: "from-amber-500 to-orange-500",
  },
  {
    name: "David Kim",
    role: "VP Eng, CloudBase",
    content:
      "The cloud migration SONTO handled for us was seamless. Zero downtime, optimized costs, and their team was incredibly responsive throughout the entire process.",
    avatar: "DK",
    gradient: "from-emerald-500 to-teal-500",
  },
  {
    name: "Lisa Thompson",
    role: "Director, NextGen",
    content:
      "SONTO's AI solutions revolutionized our data analytics capabilities. What used to take weeks now happens in real-time. Truly transformative technology.",
    avatar: "LT",
    gradient: "from-rose-500 to-red-500",
  },
];

const partners = [
  "TechCorp",
  "InnovateLab",
  "DataFlow",
  "CloudBase",
  "NextGen",
  "AlphaTech",
];

const faqs = [
  {
    q: "What services does SONTO offer?",
    a: "We offer a comprehensive range of digital services including web development, mobile app development, AI solutions, cloud services, UI/UX design, and digital strategy consulting.",
  },
  {
    q: "How long does a typical project take?",
    a: "Project timelines vary based on scope and complexity. A typical web development project takes 4-12 weeks, while larger enterprise solutions may take 3-6 months.",
  },
  {
    q: "What technologies do you use?",
    a: "We work with modern technologies including Vue.js, React, Node.js, Python, AWS, Google Cloud, TensorFlow, and many more cutting-edge tools and frameworks.",
  },
  {
    q: "Do you offer post-launch support?",
    a: "Yes, we provide comprehensive post-launch support including maintenance, updates, performance optimization, and 24/7 technical support for all our projects.",
  },
];

const currentTestimonial = ref(0);
const openFaqs = ref(new Set());
const scrollProgress = ref(0);
const isPaused = ref(false);
let autoPlayInterval = null;

const toggleFaq = (index) => {
  const newSet = new Set(openFaqs.value);
  if (newSet.has(index)) {
    newSet.delete(index);
  } else {
    newSet.add(index);
  }
  openFaqs.value = newSet;
};

const isFaqOpen = (index) => openFaqs.value.has(index);

const nextTestimonial = () => {
  currentTestimonial.value =
    (currentTestimonial.value + 1) % testimonials.length;
};

const prevTestimonial = () => {
  currentTestimonial.value =
    (currentTestimonial.value - 1 + testimonials.length) % testimonials.length;
};

const startAutoPlay = () => {
  stopAutoPlay();
  autoPlayInterval = setInterval(() => {
    if (!isPaused.value) {
      currentTestimonial.value =
        (currentTestimonial.value + 1) % testimonials.length;
    }
  }, 4000);
};

const stopAutoPlay = () => {
  if (autoPlayInterval) {
    clearInterval(autoPlayInterval);
    autoPlayInterval = null;
  }
};

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
  startAutoPlay();

  window.addEventListener("scroll", () => {
    const winScroll = document.documentElement.scrollTop;
    const height =
      document.documentElement.scrollHeight -
      document.documentElement.clientHeight;
    scrollProgress.value = (winScroll / height) * 100;
  });
});

onBeforeUnmount(() => {
  stopAutoPlay();
});
</script>

<template>
  <div class="relative">
    <!-- Scroll Progress Bar -->
    <div
      class="fixed top-20 left-0 h-0.5 bg-gradient-to-r from-primary via-secondary to-accent z-40 transition-all duration-150"
      :style="{ width: scrollProgress + '%' }"
    ></div>

    <!-- ==================== HERO SECTION ==================== -->
    <section
      class="relative min-h-screen flex items-center justify-center overflow-hidden pt-20"
    >
      <!-- Background Effects -->
      <div class="absolute inset-0 gradient-hero"></div>
      <div class="absolute inset-0 bg-grid-pattern"></div>

      <!-- Animated floating orbs -->
      <div class="absolute inset-0 overflow-hidden pointer-events-none">
        <div
          class="absolute top-1/4 left-1/4 w-72 h-72 bg-primary/20 rounded-full blur-3xl animate-float"
        ></div>
        <div
          class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-secondary/10 rounded-full blur-3xl animate-float"
          style="animation-delay: 1.5s"
        ></div>
        <div
          class="absolute top-1/3 right-1/3 w-48 h-48 bg-purple-500/10 rounded-full blur-3xl animate-float"
          style="animation-delay: 3s"
        ></div>
      </div>

      <div
        class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center"
      >
        <!-- Badge -->
        <div
          class="inline-flex items-center gap-2 px-4 py-2 rounded-full glass border border-primary/20 mb-8 animate-fade-in"
        >
          <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
          <span class="text-sm text-gray-300"
            >Empowering Digital Innovation</span
          >
        </div>

        <!-- Main Heading -->
        <h1
          class="text-5xl sm:text-6xl md:text-7xl lg:text-8xl font-extrabold leading-tight mb-6 animate-fade-in"
          style="animation-delay: 0.2s"
        >
          <span class="text-white">Digital Solutions</span>
          <br />
          <span class="gradient-text">For Tomorrow</span>
        </h1>

        <p
          class="max-w-2xl mx-auto text-lg sm:text-xl text-gray-400 mb-10 animate-fade-in"
          style="animation-delay: 0.4s"
        >
          We empower businesses with innovative technology solutions that drive
          growth, enhance efficiency, and create exceptional digital
          experiences.
        </p>

        <!-- CTA Buttons -->
        <div
          class="flex flex-col sm:flex-row items-center justify-center gap-4 animate-fade-in"
          style="animation-delay: 0.6s"
        >
          <router-link
            to="/contact"
            class="group relative px-8 py-4 rounded-xl text-lg font-semibold text-white gradient-primary overflow-hidden transition-all duration-300 hover:shadow-2xl hover:shadow-primary/30 hover:scale-105"
          >
            <span class="relative z-10">Start Your Project</span>
            <div
              class="absolute inset-0 bg-white/10 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
            ></div>
          </router-link>

          <router-link
            to="/services"
            class="group px-8 py-4 rounded-xl text-lg font-semibold text-white glass border border-white/10 hover:border-primary/30 hover:bg-white/5 transition-all duration-300"
          >
            <span class="flex items-center gap-2">
              Explore Services
              <svg
                class="w-5 h-5 group-hover:translate-x-1 transition-transform"
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
            </span>
          </router-link>
        </div>

        <!-- Stats bar -->
        <div
          class="mt-20 grid grid-cols-2 md:grid-cols-4 gap-8 max-w-3xl mx-auto animate-fade-in"
          style="animation-delay: 0.8s"
        >
          <div v-for="(stat, index) in stats" :key="index" class="text-center">
            <div class="text-3xl md:text-4xl font-bold gradient-text mb-1">
              {{ stat.value }}
            </div>
            <div class="text-sm text-gray-500">{{ stat.label }}</div>
          </div>
        </div>
      </div>

      <!-- Scroll indicator -->
      <div class="absolute bottom-8 left-1/2 -translate-x-1/2 animate-bounce">
        <svg
          class="w-6 h-6 text-gray-500"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M19 14l-7 7m0 0l-7-7m7 7V3"
          />
        </svg>
      </div>
    </section>

    <!-- ==================== SERVICES SECTION ==================== -->
    <section id="services" class="relative py-32 overflow-hidden">
      <!-- Background -->
      <div class="absolute inset-0 bg-darker"></div>
      <div
        class="absolute top-0 left-1/2 -translate-x-1/2 w-full h-px bg-gradient-to-r from-transparent via-primary/30 to-transparent"
      ></div>

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <span
            class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-primary/10 text-primary border border-primary/20 mb-6"
          >
            What We Offer
          </span>
          <h2
            class="text-4xl sm:text-5xl font-bold text-white mb-4 animate-on-scroll opacity-0"
          >
            Our
            <span class="gradient-text-purple">Services</span>
          </h2>
          <p
            class="max-w-2xl mx-auto text-gray-400 text-lg animate-on-scroll opacity-0"
          >
            Comprehensive digital solutions tailored to your business needs,
            delivered with cutting-edge technology and creative excellence.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div
            v-for="(service, index) in services"
            :key="index"
            class="group relative p-8 rounded-2xl glass border border-white/5 hover:border-primary/20 transition-all duration-500 hover:-translate-y-2 hover:shadow-xl hover:shadow-primary/5 animate-on-scroll opacity-0 cursor-pointer"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <!-- Icon -->
            <div
              class="text-4xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              {{ service.icon }}
            </div>

            <!-- Gradient line -->
            <div
              class="w-12 h-1 rounded-full bg-gradient-to-r"
              :class="service.gradient"
            ></div>

            <h3 class="text-xl font-semibold text-white mt-4 mb-3">
              {{ service.title }}
            </h3>
            <p class="text-gray-400 text-sm leading-relaxed">
              {{ service.description }}
            </p>

            <!-- Hover glow -->
            <div
              class="absolute inset-0 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-500 bg-gradient-to-b from-primary/5 to-transparent pointer-events-none"
            ></div>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== ABOUT/PORTFOLIO SECTION ==================== -->
    <section id="about" class="relative py-32 overflow-hidden">
      <div class="absolute inset-0 bg-dark"></div>
      <div
        class="absolute bottom-0 left-1/2 -translate-x-1/2 w-full h-px bg-gradient-to-r from-transparent via-secondary/30 to-transparent"
      ></div>

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-16 items-center">
          <!-- Left: Content -->
          <div>
            <span
              class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-secondary/10 text-secondary border border-secondary/20 mb-6"
            >
              About Us
            </span>
            <h2
              class="text-4xl sm:text-5xl font-bold text-white mb-6 animate-on-scroll opacity-0"
            >
              We Build
              <span class="gradient-text">Digital Excellence</span>
            </h2>
            <p
              class="text-gray-400 text-lg leading-relaxed mb-8 animate-on-scroll opacity-0"
            >
              At SONTO, we're passionate about creating digital solutions that
              make a difference. Our team of experts combines creativity with
              technical expertise to deliver exceptional results for our
              clients.
            </p>

            <div class="space-y-4 animate-on-scroll opacity-0">
              <div
                v-for="(item, index) in [
                  'Innovative & Creative Approach',
                  'Expert Team of Professionals',
                  'Client-Focused Solutions',
                  'Proven Track Record',
                ]"
                :key="index"
                class="flex items-center gap-3"
              >
                <div
                  class="w-6 h-6 rounded-full bg-gradient-to-br from-primary to-secondary flex items-center justify-center flex-shrink-0"
                >
                  <svg
                    class="w-3.5 h-3.5 text-white"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="3"
                      d="M5 13l4 4L19 7"
                    />
                  </svg>
                </div>
                <span class="text-gray-300">{{ item }}</span>
              </div>
            </div>

            <router-link
              to="/about"
              class="inline-flex items-center gap-2 mt-8 px-6 py-3 rounded-xl text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300"
            >
              Learn More About Us
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

          <!-- Right: Visual -->
          <div
            class="relative animate-on-scroll opacity-0"
            style="animation-delay: 0.3s"
          >
            <div class="relative">
              <!-- Main card -->
              <div class="relative p-8 rounded-2xl glass border border-white/5">
                <!-- Feature cards -->
                <div class="grid grid-cols-2 gap-4">
                  <div
                    v-for="(feature, index) in [
                      {
                        icon: '🚀',
                        label: 'Fast Delivery',
                        desc: 'Agile methodology',
                      },
                      {
                        icon: '🔒',
                        label: 'Secure',
                        desc: 'Enterprise grade',
                      },
                      {
                        icon: '⚡',
                        label: 'Performance',
                        desc: 'Optimized speed',
                      },
                      {
                        icon: '🎯',
                        label: 'Precision',
                        desc: 'Accurate results',
                      },
                    ]"
                    :key="index"
                    class="p-4 rounded-xl bg-white/5 border border-white/5 hover:border-primary/20 transition-all duration-300"
                  >
                    <div class="text-2xl mb-2">{{ feature.icon }}</div>
                    <h4 class="text-white font-semibold text-sm">
                      {{ feature.label }}
                    </h4>
                    <p class="text-gray-500 text-xs">{{ feature.desc }}</p>
                  </div>
                </div>
              </div>

              <!-- Decorative elements -->
              <div
                class="absolute -top-4 -right-4 w-24 h-24 bg-primary/10 rounded-full blur-xl"
              ></div>
              <div
                class="absolute -bottom-4 -left-4 w-32 h-32 bg-secondary/10 rounded-full blur-xl"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== WHY CHOOSE US SECTION ==================== -->
    <section class="relative py-32 overflow-hidden">
      <div class="absolute inset-0 bg-darker"></div>
      <div
        class="absolute top-0 left-0 right-0 h-px bg-gradient-to-r from-transparent via-primary/30 to-transparent"
      ></div>

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <span
            class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-accent/10 text-accent border border-accent/20 mb-6"
          >
            Why Choose Us
          </span>
          <h2
            class="text-4xl sm:text-5xl font-bold text-white mb-4 animate-on-scroll opacity-0"
          >
            What Makes Us
            <span class="gradient-text">Different</span>
          </h2>
          <p
            class="max-w-2xl mx-auto text-gray-400 text-lg animate-on-scroll opacity-0"
          >
            We combine technical expertise with creative vision to deliver
            solutions that exceed expectations.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div
            v-for="(item, index) in [
              {
                number: '01',
                title: 'Innovation First',
                desc: 'We stay ahead of the curve with cutting-edge technologies and creative approaches.',
                color: 'from-primary to-purple-500',
              },
              {
                number: '02',
                title: 'Quality Assured',
                desc: 'Every project undergoes rigorous testing and quality assurance processes.',
                color: 'from-secondary to-cyan-500',
              },
              {
                number: '03',
                title: 'Client Partnership',
                desc: 'We work closely with you, ensuring your vision is realized every step of the way.',
                color: 'from-amber-500 to-orange-500',
              },
            ]"
            :key="index"
            class="group relative p-8 rounded-2xl glass border border-white/5 hover:border-primary/20 transition-all duration-500 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.15 + 's' }"
          >
            <div
              class="text-5xl font-extrabold bg-gradient-to-br bg-clip-text text-transparent"
              :class="item.color"
            >
              {{ item.number }}
            </div>
            <h3 class="text-xl font-semibold text-white mt-4 mb-3">
              {{ item.title }}
            </h3>
            <p class="text-gray-400 text-sm leading-relaxed">
              {{ item.desc }}
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== TESTIMONIALS SECTION ==================== -->
    <section class="relative py-32 overflow-hidden">
      <div class="absolute inset-0 bg-dark"></div>
      <div
        class="absolute bottom-0 left-0 right-0 h-px bg-gradient-to-r from-transparent via-secondary/30 to-transparent"
      ></div>

      <!-- Decorative background -->
      <div
        class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-primary/5 rounded-full blur-3xl"
      ></div>

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <span
            class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-purple-500/10 text-purple-400 border border-purple-500/20 mb-6"
          >
            Testimonials
          </span>
          <h2
            class="text-4xl sm:text-5xl font-bold text-white mb-4 animate-on-scroll opacity-0"
          >
            What Our
            <span class="gradient-text-purple">Clients Say</span>
          </h2>
        </div>

        <!-- Modern Auto-Scroll Testimonials Carousel -->
        <div
          class="relative max-w-5xl mx-auto"
          @mouseenter="isPaused = true"
          @mouseleave="isPaused = false"
        >
          <!-- Main testimonial card -->
          <Transition name="testimonial-slide" mode="out-in">
            <div :key="currentTestimonial" class="relative">
              <div
                class="relative p-8 md:p-12 rounded-3xl glass border border-white/5 overflow-hidden group"
              >
                <!-- Gradient background accent -->
                <div
                  class="absolute -top-20 -right-20 w-40 h-40 rounded-full opacity-20 blur-2xl transition-all duration-700 group-hover:scale-150"
                  :class="testimonials[currentTestimonial].gradient"
                ></div>
                <div
                  class="absolute -bottom-20 -left-20 w-40 h-40 rounded-full opacity-10 blur-2xl transition-all duration-700 group-hover:scale-150"
                  :class="testimonials[currentTestimonial].gradient"
                ></div>

                <!-- Quote icon -->
                <div class="relative z-10 mb-6">
                  <svg
                    class="w-12 h-12 opacity-20"
                    :class="
                      'text-' +
                      testimonials[currentTestimonial].gradient
                        .split(' ')[0]
                        .replace('from-', '')
                    "
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path
                      d="M14.017 21v-7.391c0-5.704 3.731-9.57 8.983-10.609l.995 2.151c-2.432.917-3.995 3.638-3.995 5.849h4v10H14.017zM0 21v-7.391c0-5.704 3.731-9.57 8.983-10.609l.995 2.151c-2.432.917-3.995 3.638-3.995 5.849h4v10H0z"
                    />
                  </svg>
                </div>

                <!-- Content -->
                <div class="relative z-10">
                  <p
                    class="text-lg md:text-xl text-gray-200 leading-relaxed mb-8 italic font-light"
                  >
                    "{{ testimonials[currentTestimonial].content }}"
                  </p>

                  <!-- Author info -->
                  <div class="flex items-center gap-4">
                    <div
                      class="w-14 h-14 rounded-full bg-gradient-to-br p-0.5 flex-shrink-0"
                      :class="testimonials[currentTestimonial].gradient"
                    >
                      <div
                        class="w-full h-full rounded-full bg-darker flex items-center justify-center text-lg font-bold text-white"
                      >
                        {{ testimonials[currentTestimonial].avatar }}
                      </div>
                    </div>
                    <div>
                      <h4 class="text-white font-semibold text-lg">
                        {{ testimonials[currentTestimonial].name }}
                      </h4>
                      <p class="text-gray-500 text-sm">
                        {{ testimonials[currentTestimonial].role }}
                      </p>
                    </div>

                    <!-- Rating stars -->
                    <div class="ml-auto hidden sm:flex gap-1">
                      <svg
                        v-for="star in 5"
                        :key="star"
                        class="w-4 h-4 text-amber-400"
                        fill="currentColor"
                        viewBox="0 0 20 20"
                      >
                        <path
                          d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
                        />
                      </svg>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </Transition>

          <!-- Navigation controls -->
          <div class="flex items-center justify-center gap-6 mt-10">
            <!-- Prev button -->
            <button
              @click="prevTestimonial"
              class="group w-12 h-12 rounded-full glass border border-white/5 flex items-center justify-center text-gray-400 hover:text-white hover:border-primary/30 hover:bg-primary/10 transition-all duration-300 hover:scale-110"
              aria-label="Previous testimonial"
            >
              <svg
                class="w-5 h-5 group-hover:-translate-x-0.5 transition-transform"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M15 19l-7-7 7-7"
                />
              </svg>
            </button>

            <!-- Dots -->
            <div class="flex gap-3">
              <button
                v-for="(_, index) in testimonials"
                :key="index"
                @click="currentTestimonial = index"
                class="relative transition-all duration-500 rounded-full"
                :class="
                  index === currentTestimonial
                    ? 'w-10 h-2.5 bg-gradient-to-r ' +
                      testimonials[index].gradient
                    : 'w-2.5 h-2.5 bg-white/20 hover:bg-white/40'
                "
              >
                <span
                  v-if="index === currentTestimonial"
                  class="absolute inset-0 rounded-full animate-pulse opacity-40"
                  :class="'bg-gradient-to-r ' + testimonials[index].gradient"
                ></span>
              </button>
            </div>

            <!-- Next button -->
            <button
              @click="nextTestimonial"
              class="group w-12 h-12 rounded-full glass border border-white/5 flex items-center justify-center text-gray-400 hover:text-white hover:border-primary/30 hover:bg-primary/10 transition-all duration-300 hover:scale-110"
              aria-label="Next testimonial"
            >
              <svg
                class="w-5 h-5 group-hover:translate-x-0.5 transition-transform"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M9 5l7 7-7 7"
                />
              </svg>
            </button>
          </div>

          <!-- Auto-play indicator -->
          <div class="flex items-center justify-center gap-2 mt-6">
            <div class="flex gap-1">
              <div
                v-for="(_, index) in testimonials"
                :key="index"
                class="h-1 rounded-full transition-all duration-300"
                :class="
                  index <= currentTestimonial
                    ? 'bg-primary/30 w-4'
                    : 'bg-white/5 w-4'
                "
              ></div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== PARTNERS SECTION ==================== -->
    <section class="relative py-20 overflow-hidden bg-darker">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <p
          class="text-center text-gray-500 text-sm uppercase tracking-widest mb-10"
        >
          Trusted by Industry Leaders
        </p>
        <div
          class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-8 items-center justify-items-center"
        >
          <div
            v-for="(partner, index) in partners"
            :key="index"
            class="text-gray-600 hover:text-gray-400 text-xl font-bold transition-colors duration-300 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            {{ partner }}
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== FAQ SECTION ==================== -->
    <section class="relative py-32 overflow-hidden">
      <div class="absolute inset-0 bg-dark"></div>

      <!-- Decorative gradient blobs -->
      <div
        class="absolute top-1/3 left-0 w-96 h-96 bg-primary/5 rounded-full blur-3xl pointer-events-none"
      ></div>
      <div
        class="absolute bottom-1/3 right-0 w-80 h-80 bg-secondary/5 rounded-full blur-3xl pointer-events-none"
      ></div>

      <div class="relative max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <span
            class="inline-flex items-center gap-2 px-4 py-2 rounded-full text-sm font-medium bg-primary/10 text-primary border border-primary/20 mb-6"
          >
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
                d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            Got Questions?
          </span>
          <h2
            class="text-4xl sm:text-5xl font-bold text-white mb-4 animate-on-scroll opacity-0"
          >
            Frequently Asked
            <span class="gradient-text">Questions</span>
          </h2>
          <p
            class="max-w-2xl mx-auto text-gray-400 text-lg animate-on-scroll opacity-0"
          >
            Find answers to common questions about our services, process, and
            how we can help bring your vision to life.
          </p>
        </div>

        <div class="space-y-3">
          <div
            v-for="(faq, index) in faqs"
            :key="index"
            class="group rounded-2xl transition-all duration-400 animate-on-scroll opacity-0"
            :class="
              isFaqOpen(index)
                ? 'bg-gradient-to-br from-primary/10 via-dark to-secondary/5 border border-primary/20 shadow-lg shadow-primary/10'
                : 'bg-white/[0.03] border border-white/[0.06] hover:bg-white/[0.05] hover:border-white/[0.12]'
            "
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <button
              @click="toggleFaq(index)"
              class="w-full flex items-center justify-between gap-4 p-5 md:p-6 text-left"
              :aria-expanded="isFaqOpen(index).toString()"
            >
              <div class="flex items-start gap-4">
                <!-- Index number badge -->
                <span
                  class="hidden sm:flex items-center justify-center w-8 h-8 rounded-lg mt-0.5 text-sm font-bold transition-all duration-300 flex-shrink-0"
                  :class="
                    isFaqOpen(index)
                      ? 'bg-primary/20 text-primary'
                      : 'bg-white/5 text-gray-500 group-hover:bg-white/10 group-hover:text-gray-300'
                  "
                >
                  0{{ index + 1 }}
                </span>
                <span
                  class="text-base md:text-lg font-medium pr-2 transition-all duration-300"
                  :class="
                    isFaqOpen(index)
                      ? 'text-white'
                      : 'text-gray-300 group-hover:text-white'
                  "
                >
                  {{ faq.q }}
                </span>
              </div>
              <!-- Animated arrow icon -->
              <div
                class="relative w-9 h-9 rounded-full flex items-center justify-center flex-shrink-0 transition-all duration-300"
                :class="
                  isFaqOpen(index)
                    ? 'bg-primary/20 text-primary shadow-sm shadow-primary/20'
                    : 'bg-white/5 text-gray-500 group-hover:bg-white/10 group-hover:text-gray-300'
                "
              >
                <svg
                  class="w-4 h-4 transition-all duration-400"
                  :class="isFaqOpen(index) ? 'rotate-180' : ''"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2.5"
                    d="M19 9l-7 7-7-7"
                  />
                </svg>
              </div>
            </button>

            <!-- Answer content with expand/collapse animation -->
            <div
              class="transition-all duration-400 ease-[cubic-bezier(0.4,0,0.2,1)]"
              :class="
                isFaqOpen(index)
                  ? 'max-h-[500px] opacity-100'
                  : 'max-h-0 opacity-0 overflow-hidden'
              "
            >
              <div class="px-5 md:px-6 pb-5 md:pb-6">
                <div
                  class="h-px w-full bg-gradient-to-r from-primary/20 via-secondary/10 to-transparent mb-4"
                ></div>
                <p class="text-sm md:text-base leading-relaxed text-gray-300">
                  {{ faq.a }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Bottom CTA -->
        <div class="text-center mt-12 animate-on-scroll opacity-0">
          <p class="text-gray-500 text-sm mb-4">Still have questions?</p>
          <router-link
            to="/contact"
            class="inline-flex items-center gap-2 px-6 py-3 rounded-xl text-sm font-semibold text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300 hover:scale-105"
          >
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
                d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
              />
            </svg>
            Contact Us
          </router-link>
        </div>
      </div>
    </section>

    <!-- ==================== CTA SECTION ==================== -->
    <section class="relative py-32 overflow-hidden">
      <div class="absolute inset-0 bg-darker"></div>
      <div
        class="absolute top-0 left-0 right-0 h-px bg-gradient-to-r from-transparent via-primary/30 to-transparent"
      ></div>

      <div class="relative max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <!-- Background glow -->
        <div
          class="absolute inset-0 bg-primary/5 rounded-full blur-3xl opacity-50"
        ></div>

        <div class="relative">
          <h2
            class="text-4xl sm:text-5xl font-bold text-white mb-6 animate-on-scroll opacity-0"
          >
            Ready to Transform Your
            <span class="gradient-text">Digital Presence</span>?
          </h2>
          <p
            class="max-w-2xl mx-auto text-gray-400 text-lg mb-10 animate-on-scroll opacity-0"
          >
            Let's discuss how we can help your business thrive in the digital
            age. Get in touch with us today.
          </p>
          <div
            class="flex flex-col sm:flex-row items-center justify-center gap-4 animate-on-scroll opacity-0"
          >
            <router-link
              to="/contact"
              class="group relative px-8 py-4 rounded-xl text-lg font-semibold text-white gradient-primary overflow-hidden transition-all duration-300 hover:shadow-2xl hover:shadow-primary/30 hover:scale-105"
            >
              <span class="relative z-10">Get In Touch</span>
              <div
                class="absolute inset-0 bg-white/10 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
              ></div>
            </router-link>
            <router-link
              to="/services"
              class="group px-8 py-4 rounded-xl text-lg font-semibold text-white glass border border-white/10 hover:border-primary/30 hover:bg-white/5 transition-all duration-300"
            >
              View Our Services
            </router-link>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
/* Animate elements when they scroll into view using data-animated attribute */
.animate-on-scroll {
  opacity: 0;
}

.animate-on-scroll[data-animated="true"] {
  animation: fade-in 0.6s ease-out forwards;
}

/* Testimonials slide transition */
.testimonial-slide-enter-active {
  animation: testimonial-enter 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}
.testimonial-slide-leave-active {
  animation: testimonial-leave 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes testimonial-enter {
  from {
    opacity: 0;
    transform: translateX(60px) scale(0.95);
    filter: blur(4px);
  }
  to {
    opacity: 1;
    transform: translateX(0) scale(1);
    filter: blur(0);
  }
}

@keyframes testimonial-leave {
  from {
    opacity: 1;
    transform: translateX(0) scale(1);
    filter: blur(0);
  }
  to {
    opacity: 0;
    transform: translateX(-60px) scale(0.95);
    filter: blur(4px);
  }
}
</style>
