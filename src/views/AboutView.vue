<script setup>
import { onMounted } from "vue";

const team = [
  {
    name: "Ahmed Hassan",
    role: "CEO & Founder",
    avatar: "AH",
    gradient: "from-primary to-purple-500",
    bio: "Visionary leader with 15+ years in digital transformation",
  },
  {
    name: "Mariam Ali",
    role: "CTO",
    avatar: "MA",
    gradient: "from-secondary to-cyan-500",
    bio: "Technical architect specializing in scalable systems",
  },
  {
    name: "Youssef Ibrahim",
    role: "Lead Designer",
    avatar: "YI",
    gradient: "from-amber-500 to-orange-500",
    bio: "Award-winning designer passionate about user experience",
  },
  {
    name: "Nour El-Din",
    role: "Head of AI",
    avatar: "NE",
    gradient: "from-purple-500 to-pink-500",
    bio: "AI researcher with expertise in machine learning",
  },
  {
    name: "Sara Mostafa",
    role: "Project Manager",
    avatar: "SM",
    gradient: "from-emerald-500 to-teal-500",
    bio: "Certified PMP with a track record of successful deliveries",
  },
  {
    name: "Khaled Omar",
    role: "Lead Developer",
    avatar: "KO",
    gradient: "from-rose-500 to-red-500",
    bio: "Full-stack developer specializing in modern web technologies",
  },
];

const milestones = [
  {
    year: "2021",
    title: "Company Founded",
    desc: "SONTO was born with a vision to transform digital experiences",
  },
  {
    year: "2022",
    title: "First 50 Clients",
    desc: "Reached milestone of serving 50+ happy clients worldwide",
  },
  {
    year: "2023",
    title: "Team Expansion",
    desc: "Grew to 15+ talented professionals across multiple disciplines",
  },
  {
    year: "2024",
    title: "AI Division Launch",
    desc: "Launched dedicated AI and machine learning practice",
  },
  {
    year: "2025",
    title: "Global Reach",
    desc: "Expanded operations to serve clients across 20+ countries",
  },
];

const values = [
  {
    icon: "🎯",
    title: "Excellence",
    desc: "We strive for the highest quality in everything we deliver",
  },
  {
    icon: "💡",
    title: "Innovation",
    desc: "We embrace creativity and cutting-edge technology",
  },
  {
    icon: "🤝",
    title: "Partnership",
    desc: "We build lasting relationships based on trust and results",
  },
  {
    icon: "🌍",
    title: "Impact",
    desc: "We create solutions that make a real difference",
  },
];

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
    <!-- Hero -->
    <section class="relative py-20 overflow-hidden">
      <div class="absolute inset-0 gradient-hero"></div>
      <div class="absolute inset-0 bg-grid-pattern"></div>
      <div
        class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-secondary/10 rounded-full blur-3xl"
      ></div>

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h1 class="text-5xl md:text-6xl font-bold text-white mb-6">
          About
          <span class="gradient-text">SONTO</span>
        </h1>
        <p class="max-w-3xl mx-auto text-gray-400 text-lg leading-relaxed">
          We are a team of passionate technologists, designers, and strategists
          dedicated to helping businesses thrive in the digital age. Founded in
          2021, we've grown from a small startup to a trusted digital solutions
          partner.
        </p>
      </div>
    </section>

    <!-- Story -->
    <section class="py-20 bg-darker">
      <div
        class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 lg:grid-cols-2 gap-16 items-center"
      >
        <div>
          <span
            class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-primary/10 text-primary border border-primary/20 mb-6"
          >
            Our Story
          </span>
          <h2
            class="text-4xl font-bold text-white mb-6 animate-on-scroll opacity-0"
          >
            From Vision to
            <span class="gradient-text-purple">Reality</span>
          </h2>
          <p
            class="text-gray-400 leading-relaxed mb-6 animate-on-scroll opacity-0"
          >
            SONTO started with a simple belief: that technology has the power to
            transform businesses and improve lives. What began as a small team
            of three has grown into a dynamic company of 15+ experts, serving
            clients across the globe.
          </p>
          <p class="text-gray-400 leading-relaxed animate-on-scroll opacity-0">
            Our journey has been defined by continuous learning, adaptation, and
            an unwavering commitment to delivering excellence. Every project we
            undertake is an opportunity to push boundaries and create something
            extraordinary.
          </p>
        </div>

        <!-- Milestones -->
        <div class="space-y-6">
          <div
            v-for="(milestone, index) in milestones"
            :key="index"
            class="relative pl-8 border-l-2 border-white/5 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <div
              class="absolute -left-[9px] top-0 w-4 h-4 rounded-full bg-gradient-to-br from-primary to-secondary"
            ></div>
            <span class="text-sm text-primary font-semibold">{{
              milestone.year
            }}</span>
            <h3 class="text-white font-semibold mt-1">
              {{ milestone.title }}
            </h3>
            <p class="text-gray-500 text-sm">{{ milestone.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Values -->
    <section class="py-20 bg-dark">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <span
            class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-accent/10 text-accent border border-accent/20 mb-6"
          >
            Our Values
          </span>
          <h2 class="text-4xl font-bold text-white mb-4">
            What We Stand
            <span class="gradient-text">For</span>
          </h2>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          <div
            v-for="(value, index) in values"
            :key="index"
            class="p-8 rounded-2xl glass border border-white/5 hover:border-primary/20 transition-all duration-500 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <div class="text-4xl mb-4">{{ value.icon }}</div>
            <h3 class="text-lg font-semibold text-white mb-2">
              {{ value.title }}
            </h3>
            <p class="text-gray-400 text-sm">{{ value.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Team -->
    <section class="py-20 bg-darker">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <span
            class="inline-block px-4 py-2 rounded-full text-sm font-medium bg-purple-500/10 text-purple-400 border border-purple-500/20 mb-6"
          >
            Our Team
          </span>
          <h2 class="text-4xl font-bold text-white mb-4">
            Meet the
            <span class="gradient-text-purple">Experts</span>
          </h2>
          <p class="text-gray-400 text-lg max-w-2xl mx-auto">
            Talented professionals dedicated to bringing your vision to life
          </p>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            v-for="(member, index) in team"
            :key="index"
            class="group p-8 rounded-2xl glass border border-white/5 hover:border-primary/20 transition-all duration-500 animate-on-scroll opacity-0"
            :style="{ animationDelay: index * 0.1 + 's' }"
          >
            <div
              class="w-20 h-20 mx-auto mb-6 rounded-full bg-gradient-to-br p-0.5"
              :class="member.gradient"
            >
              <div
                class="w-full h-full rounded-full bg-darker flex items-center justify-center text-2xl font-bold text-white"
              >
                {{ member.avatar }}
              </div>
            </div>
            <h3 class="text-lg font-semibold text-white text-center">
              {{ member.name }}
            </h3>
            <p class="text-primary text-sm text-center mb-3">
              {{ member.role }}
            </p>
            <p class="text-gray-500 text-sm text-center">{{ member.bio }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section class="py-20 bg-dark">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h2 class="text-4xl font-bold text-white mb-6">
          Want to Join Our
          <span class="gradient-text">Team</span>?
        </h2>
        <p class="text-gray-400 text-lg mb-8">
          We're always looking for talented individuals who share our passion
          for digital innovation.
        </p>
        <router-link
          to="/contact"
          class="inline-flex items-center gap-2 px-8 py-4 rounded-xl text-lg font-semibold text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300"
        >
          Get in Touch
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
