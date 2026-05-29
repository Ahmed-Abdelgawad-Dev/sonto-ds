<script setup>
import { ref, onMounted, onBeforeUnmount, nextTick } from "vue";
import { useRouter } from "vue-router";
import gsap from "gsap";
import Typed from "typed.js";

const router = useRouter();
const animatedElements = ref([]);
const canvasRef = ref(null);
const servicesCanvasRef = ref(null);
const aboutCanvasRef = ref(null);
const whyCanvasRef = ref(null);
const testimonialCanvasRef = ref(null);
const faqCanvasRef = ref(null);
const ctaCanvasRef = ref(null);
const heroContentRef = ref(null);
const typedHeadingRef = ref(null);
let typedInstance = null;
let typedTimer = null;
let animationTween = null;
let particles = [];
const animCleanups = [];

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
    icon: "📧",
    title: "Email Hosting",
    description:
      "Reliable and secure email hosting solutions with advanced spam protection, custom domains, and seamless collaboration tools to keep your business communications professional.",
    gradient: "from-green-500 to-emerald-500",
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
    name: "Ahmed Abdel Ghaffar",
    role: "CEO, SONTO",
    content:
      "SONTO transformed our digital presence completely. Their team's expertise in web development and AI solutions is unmatched. We saw a 200% increase in engagement within months.",
    avatar: "AG",
    gradient: "from-primary to-purple-500",
  },
  {
    name: "Sara Mourad",
    role: "CTO, TechFlow",
    content:
      "Working with SONTO was a game-changer for our business. Their cloud solutions helped us scale efficiently while reducing costs by 40%.",
    avatar: "SM",
    gradient: "from-secondary to-cyan-500",
  },
  {
    name: "Khaleed Al-Nami",
    role: "Founder, CreativeLab",
    content:
      "The UI/UX design team at SONTO truly understands user experience. They created an intuitive interface that our customers love. Professional, creative, and reliable.",
    avatar: "KA",
    gradient: "from-amber-500 to-orange-500",
  },
  {
    name: "Noura Al-Sa'id",
    role: "VP Engineering, CloudBase",
    content:
      "The cloud migration SONTO handled for us was seamless. Zero downtime, optimized costs, and their team was incredibly responsive throughout the entire process.",
    avatar: "NA",
    gradient: "from-emerald-500 to-teal-500",
  },
  {
    name: "Ahmed Abdel Hamid",
    role: "Director, NextGen",
    content:
      "SONTO's AI solutions revolutionized our data analytics capabilities. What used to take weeks now happens in real-time. Truly transformative technology.",
    avatar: "AA",
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
    a: "We work with modern technologies including Vue.js, React, Node.js, Python, AWS, Django Framework, TensorFlow, and many more cutting-edge tools and frameworks.",
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

const initSectionCanvas = (canvasRef, initFn) => {
  const canvas = canvasRef.value;
  if (!canvas) return null;
  const ctx = canvas.getContext("2d");
  const parent = canvas.parentElement;
  const w = () => parent.offsetWidth;
  const h = () => parent.offsetHeight;
  canvas.width = w();
  canvas.height = h();

  // Mouse tracking
  const mouse = { x: -9999, y: -9999 };
  const handleMouseMove = (e) => {
    const rect = parent.getBoundingClientRect();
    mouse.x = e.clientX - rect.left;
    mouse.y = e.clientY - rect.top;
  };
  const handleMouseLeave = () => {
    mouse.x = -9999;
    mouse.y = -9999;
  };
  parent.addEventListener("mousemove", handleMouseMove);
  parent.addEventListener("mouseleave", handleMouseLeave);

  const state = initFn(ctx, w, h, canvas, mouse);
  const animate = () => {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    if (state.animate) state.animate(ctx, w, h, mouse);
  };

  const ticker = gsap.ticker.add(animate);

  const handleResize = () => {
    canvas.width = w();
    canvas.height = h();
    if (state.resize) state.resize(canvas, w, h);
  };
  window.addEventListener("resize", handleResize);

  return {
    ticker,
    resizeHandler: handleResize,
    mouseCleanup: () => {
      parent.removeEventListener("mousemove", handleMouseMove);
      parent.removeEventListener("mouseleave", handleMouseLeave);
    },
  };
};

/* ─── 1. HERO: Particle Network ─── */
const initHero = (ctx, w, h, canvas, mouse) => {
  const PARTICLE_COUNT = 60;
  const CONNECTION_DIST = 150;
  const colors = ["99, 102, 241", "6, 182, 212", "167, 139, 250"];
  const particles = [];

  for (let i = 0; i < PARTICLE_COUNT; i++) {
    particles.push({
      x: Math.random() * w(),
      y: Math.random() * h(),
      size: Math.random() * 2.5 + 1,
      color: colors[Math.floor(Math.random() * colors.length)],
      vx: (Math.random() - 0.5) * 0.4,
      vy: (Math.random() - 0.5) * 0.4,
      opacity: Math.random() * 0.5 + 0.3,
      baseSize: Math.random() * 2.5 + 1,
    });
  }

  return {
    animate: (ctx, w, h, mouse) => {
      particles.forEach((p) => {
        // Mouse interaction: attract and enlarge near mouse
        const dx = mouse.x - p.x;
        const dy = mouse.y - p.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < 200) {
          const force = (1 - dist / 200) * 0.3;
          p.vx += dx * force * 0.01;
          p.vy += dy * force * 0.01;
          p.size = p.baseSize + (1 - dist / 200) * 4;
          p.opacity = Math.min(1, p.opacity + (1 - dist / 200) * 0.4);
        } else {
          p.size += (p.baseSize - p.size) * 0.05;
          p.opacity += (Math.random() * 0.5 + 0.3 - p.opacity) * 0.02;
        }

        p.vx *= 0.98;
        p.vy *= 0.98;
        p.x += p.vx;
        p.y += p.vy;
        if (p.x < 0 || p.x > w()) p.vx *= -1;
        if (p.y < 0 || p.y > h()) p.vy *= -1;
        p.x = Math.max(0, Math.min(w(), p.x));
        p.y = Math.max(0, Math.min(h(), p.y));

        ctx.beginPath();
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${p.color}, ${p.opacity})`;
        ctx.fill();
      });

      for (let i = 0; i < particles.length; i++) {
        for (let j = i + 1; j < particles.length; j++) {
          const dx = particles[i].x - particles[j].x;
          const dy = particles[i].y - particles[j].y;
          const dist = Math.sqrt(dx * dx + dy * dy);
          if (dist < CONNECTION_DIST) {
            const alpha = (1 - dist / CONNECTION_DIST) * 0.35;
            ctx.beginPath();
            ctx.moveTo(particles[i].x, particles[i].y);
            ctx.lineTo(particles[j].x, particles[j].y);
            ctx.strokeStyle = `rgba(99, 102, 241, ${alpha})`;
            ctx.lineWidth = 0.8;
            ctx.stroke();
          }
        }
      }
    },
  };
};

/* ─── 2. SERVICES: Floating Geometric Shapes ─── */
const initServices = (ctx, w, h, canvas, mouse) => {
  const shapes = [];
  const colors = [
    "rgba(99,102,241,",
    "rgba(6,182,212,",
    "rgba(167,139,250,",
    "rgba(244,63,94,",
  ];
  const types = ["hex", "tri", "diamond"];

  for (let i = 0; i < 20; i++) {
    shapes.push({
      x: Math.random() * w(),
      y: Math.random() * h(),
      size: Math.random() * 12 + 4,
      type: types[Math.floor(Math.random() * types.length)],
      color: colors[Math.floor(Math.random() * colors.length)],
      rotation: Math.random() * Math.PI * 2,
      rotSpeed: (Math.random() - 0.5) * 0.015,
      vx: (Math.random() - 0.5) * 0.15,
      vy: (Math.random() - 0.5) * 0.15,
      opacity: Math.random() * 0.15 + 0.06,
      baseSize: Math.random() * 12 + 4,
    });
  }

  const drawHex = (ctx, x, y, r, rot) => {
    ctx.beginPath();
    for (let i = 0; i < 6; i++) {
      const angle = rot + (Math.PI / 3) * i;
      const px = x + r * Math.cos(angle);
      const py = y + r * Math.sin(angle);
      i === 0 ? ctx.moveTo(px, py) : ctx.lineTo(px, py);
    }
    ctx.closePath();
    ctx.fill();
  };

  const drawTri = (ctx, x, y, r, rot) => {
    ctx.beginPath();
    for (let i = 0; i < 3; i++) {
      const angle = rot + ((Math.PI * 2) / 3) * i;
      const px = x + r * Math.cos(angle);
      const py = y + r * Math.sin(angle);
      i === 0 ? ctx.moveTo(px, py) : ctx.lineTo(px, py);
    }
    ctx.closePath();
    ctx.fill();
  };

  const drawDiamond = (ctx, x, y, r, rot) => {
    ctx.save();
    ctx.translate(x, y);
    ctx.rotate(rot);
    ctx.beginPath();
    ctx.moveTo(0, -r);
    ctx.lineTo(r * 0.6, 0);
    ctx.lineTo(0, r);
    ctx.lineTo(-r * 0.6, 0);
    ctx.closePath();
    ctx.fill();
    ctx.restore();
  };

  return {
    animate: (ctx, w, h, mouse) => {
      shapes.forEach((s) => {
        // Mouse interaction: rotate faster & glow near mouse
        const dx = mouse.x - s.x;
        const dy = mouse.y - s.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < 180) {
          const force = (1 - dist / 180) * 0.5;
          s.rotSpeed += force * 0.01;
          s.opacity = Math.min(0.4, (s.baseSize / 16) * 0.15 + force * 0.2);
          s.size = s.baseSize + force * 6;
        } else {
          s.rotSpeed +=
            (shapes.indexOf(s) % 2 === 0 ? 0.015 : -0.015 - s.rotSpeed) * 0.01;
          s.size += (s.baseSize - s.size) * 0.05;
          s.opacity += ((s.baseSize / 16) * 0.15 + 0.06 - s.opacity) * 0.02;
        }

        s.x += s.vx;
        s.y += s.vy;
        s.rotation += s.rotSpeed;
        if (s.x < -20 || s.x > w() + 20) s.vx *= -1;
        if (s.y < -20 || s.y > h() + 20) s.vy *= -1;

        ctx.fillStyle = `${s.color}${s.opacity})`;
        if (s.type === "hex") drawHex(ctx, s.x, s.y, s.size, s.rotation);
        else if (s.type === "tri") drawTri(ctx, s.x, s.y, s.size, s.rotation);
        else drawDiamond(ctx, s.x, s.y, s.size, s.rotation);
      });
    },
    resize: (canvas, w, h) => {
      shapes.forEach((s) => {
        if (s.x > w()) s.x = w() - 10;
        if (s.y > h()) s.y = h() - 10;
      });
    },
  };
};

/* ─── 3. ABOUT: Data Flow Sine Waves ─── */
const initAbout = (ctx, w, h, canvas, mouse) => {
  const waves = [];
  const colors = ["99, 102, 241", "6, 182, 212", "139, 92, 246"];

  for (let i = 0; i < 5; i++) {
    waves.push({
      amplitude: Math.random() * 15 + 8,
      frequency: 0.008 + Math.random() * 0.006,
      speed: 0.005 + Math.random() * 0.01,
      phase: Math.random() * Math.PI * 2,
      y: h() * (0.12 + i * 0.18),
      color: colors[i % colors.length],
      lineWidth: 1 + Math.random() * 1.5,
      opacity: 0.08 + Math.random() * 0.08,
      mouseInfluence: 0,
    });
  }

  return {
    animate: (ctx, w, h, mouse) => {
      waves.forEach((wave) => {
        const distToMouse = Math.abs(mouse.y - wave.y);
        const mouseBoost =
          distToMouse < 150 ? (1 - distToMouse / 150) * 1.5 : 0;
        wave.opacity = Math.min(
          0.3,
          0.08 + Math.random() * 0.08 + mouseBoost * 0.08,
        );

        ctx.beginPath();
        ctx.strokeStyle = `rgba(${wave.color}, ${wave.opacity})`;
        ctx.lineWidth = wave.lineWidth + mouseBoost * 0.5;
        for (let x = 0; x < w(); x++) {
          const extraAmp =
            Math.max(0, 1 - Math.abs(mouse.x - x) / 200) * mouseBoost * 10;
          const y =
            wave.y +
            Math.sin(x * wave.frequency + wave.phase) *
              (wave.amplitude + extraAmp);
          x === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
        }
        ctx.stroke();
        wave.phase += wave.speed;

        ctx.beginPath();
        ctx.strokeStyle = `rgba(${wave.color}, ${wave.opacity * 0.4})`;
        ctx.lineWidth = wave.lineWidth * 0.5 + mouseBoost * 0.3;
        for (let x = 0; x < w(); x++) {
          const extraAmp =
            Math.max(0, 1 - Math.abs(mouse.x - x) / 200) * mouseBoost * 6;
          const y =
            wave.y +
            Math.sin(x * wave.frequency * 1.5 + wave.phase * 1.3) *
              (wave.amplitude * 0.7 + extraAmp);
          x === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
        }
        ctx.stroke();
      });
    },
  };
};

/* ─── 4. WHY CHOOSE US: Rising Ember Particles ─── */
const initWhyChoose = (ctx, w, h, canvas, mouse) => {
  const embers = [];
  const colors = [
    "99, 102, 241",
    "6, 182, 212",
    "245, 158, 11",
    "167, 139, 250",
  ];

  for (let i = 0; i < 35; i++) {
    embers.push(createEmber(w(), h(), colors));
  }

  function createEmber(cw, ch, cols) {
    return {
      x: Math.random() * cw,
      y: Math.random() * ch,
      size: Math.random() * 2 + 0.5,
      color: cols[Math.floor(Math.random() * cols.length)],
      vy: -(Math.random() * 0.3 + 0.15),
      vx: (Math.random() - 0.5) * 0.2,
      opacity: Math.random() * 0.3 + 0.1,
      pulse: Math.random() * Math.PI * 2,
      pulseSpeed: 0.01 + Math.random() * 0.02,
      baseSize: Math.random() * 2 + 0.5,
    };
  }

  return {
    animate: (ctx, w, h, mouse) => {
      embers.forEach((e) => {
        // Mouse: attract and brighten
        const dx = mouse.x - e.x;
        const dy = mouse.y - e.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < 150) {
          const force = (1 - dist / 150) * 0.4;
          e.vx += dx * force * 0.005;
          e.vy += dy * force * 0.005;
          e.size = e.baseSize + force * 4;
        } else {
          e.size += (e.baseSize - e.size) * 0.05;
        }

        e.y += e.vy;
        e.x += e.vx;
        e.pulse += e.pulseSpeed;
        e.vx *= 0.99;
        e.vy *= 0.99;

        const pulseOpacity = Math.min(
          1,
          e.opacity * (0.6 + 0.4 * Math.sin(e.pulse)),
        );
        if (e.y < -10) {
          Object.assign(e, createEmber(w(), h(), colors));
          e.y = h() + 10;
        }
        if (e.x < -10 || e.x > w() + 10) e.vx *= -1;

        ctx.beginPath();
        ctx.arc(e.x, e.y, e.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${e.color}, ${pulseOpacity})`;
        ctx.fill();
        ctx.beginPath();
        ctx.arc(e.x, e.y, e.size * 3, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${e.color}, ${pulseOpacity * 0.2})`;
        ctx.fill();
      });
    },
  };
};

/* ─── 5. TESTIMONIALS: Orbiting Rings ─── */
const initTestimonials = (ctx, w, h, canvas, mouse) => {
  const systems = [];
  const colors = ["99, 102, 241", "6, 182, 212", "167, 139, 250"];

  for (let s = 0; s < 3; s++) {
    const cx = w() * (0.2 + s * 0.3);
    const cy = h() * (0.3 + Math.random() * 0.4);
    const rings = [];
    for (let r = 0; r < 2; r++) {
      const orbitR = 30 + r * 25 + Math.random() * 15;
      const dots = [];
      for (let d = 0; d < 5 + Math.floor(Math.random() * 4); d++) {
        dots.push({
          angle: Math.random() * Math.PI * 2,
          speed: (Math.random() * 0.005 + 0.003) * (r % 2 === 0 ? 1 : -1),
          size: Math.random() * 2 + 1.5,
          opacity: Math.random() * 0.2 + 0.1,
          baseSize: Math.random() * 2 + 1.5,
        });
      }
      rings.push({ orbitR, dots, color: colors[(s + r) % colors.length] });
    }
    systems.push({ cx, cy, rings });
  }

  return {
    animate: (ctx, w, h, mouse) => {
      systems.forEach((sys) => {
        const dx = mouse.x - sys.cx;
        const dy = mouse.y - sys.cy;
        const dist = Math.sqrt(dx * dx + dy * dy);
        const nearMouse = dist < 200;
        const force = nearMouse ? (1 - dist / 200) * 1.5 : 0;

        sys.rings.forEach((ring) => {
          ctx.beginPath();
          ctx.arc(sys.cx, sys.cy, ring.orbitR + force * 10, 0, Math.PI * 2);
          ctx.strokeStyle = `rgba(${ring.color}, ${0.04 + force * 0.06})`;
          ctx.lineWidth = 0.5 + force * 0.5;
          ctx.stroke();

          ring.dots.forEach((dot) => {
            dot.angle += dot.speed + (nearMouse ? force * 0.003 : 0);
            const r = ring.orbitR + (nearMouse ? force * 8 : 0);
            const x = sys.cx + Math.cos(dot.angle) * r;
            const y = sys.cy + Math.sin(dot.angle) * r;
            const sizeBoost = nearMouse ? force * 2 : 0;
            ctx.beginPath();
            ctx.arc(x, y, dot.baseSize + sizeBoost, 0, Math.PI * 2);
            ctx.fillStyle = `rgba(${ring.color}, ${Math.min(1, dot.opacity + force * 0.1)})`;
            ctx.fill();
          });
        });
      });
    },
    resize: (canvas, w, h) => {
      systems.forEach((sys, i) => {
        sys.cx = w() * (0.2 + i * 0.3);
        sys.cy = h() * (0.3 + Math.random() * 0.1);
      });
    },
  };
};

/* ─── 6. FAQ: Fluid Morphing Blobs ─── */
const initFaq = (ctx, w, h, canvas, mouse) => {
  const blobs = [];
  for (let i = 0; i < 3; i++) {
    blobs.push({
      x: w() * (0.2 + i * 0.3),
      y: h() * (0.3 + Math.random() * 0.4),
      radius: 40 + Math.random() * 30,
      time: Math.random() * Math.PI * 2,
      speed: 0.003 + Math.random() * 0.003,
      deformX: Math.random() * 0.4 + 0.3,
      deformY: Math.random() * 0.4 + 0.3,
      color: ["99, 102, 241", "6, 182, 212", "167, 139, 250"][i],
      opacity: 0.06 + Math.random() * 0.04,
      baseRadius: 40 + Math.random() * 30,
    });
  }

  return {
    animate: (ctx, w, h, mouse) => {
      blobs.forEach((b) => {
        b.time += b.speed;
        const dx = mouse.x - b.x;
        const dy = mouse.y - b.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        const nearMouse = dist < 180;
        const force = nearMouse ? (1 - dist / 180) * 0.5 : 0;

        b.radius = b.baseRadius + force * 15;
        b.opacity = Math.min(0.3, 0.06 + Math.random() * 0.04 + force * 0.12);

        if (nearMouse) {
          b.x += (mouse.x - b.x) * 0.005;
          b.y += (mouse.y - b.y) * 0.005;
        }

        const points = 20;
        ctx.beginPath();
        for (let i = 0; i <= points; i++) {
          const angle = ((Math.PI * 2) / points) * i;
          const r =
            b.radius +
            Math.sin(angle * 3 + b.time) * b.deformX * (15 + force * 10) +
            Math.cos(angle * 2 + b.time * 1.3) * b.deformY * (12 + force * 8);
          const px = b.x + Math.cos(angle) * r;
          const py = b.y + Math.sin(angle) * r;
          i === 0 ? ctx.moveTo(px, py) : ctx.lineTo(px, py);
        }
        ctx.closePath();
        ctx.fillStyle = `rgba(${b.color}, ${b.opacity})`;
        ctx.fill();
      });
    },
  };
};

/* ─── 7. CTA: Pulsing Energy Rings ─── */
const initCta = (ctx, w, h, canvas, mouse) => {
  const rings = [];
  const colors = ["99, 102, 241", "6, 182, 212"];

  for (let i = 0; i < 8; i++) {
    rings.push({
      x: Math.random() * w(),
      y: Math.random() * h(),
      radius: 0,
      maxRadius: 40 + Math.random() * 60,
      speed: 0.15 + Math.random() * 0.2,
      color: colors[i % colors.length],
      opacity: 0.15,
      phase: Math.random() * Math.PI * 2,
      baseMax: 40 + Math.random() * 60,
    });
  }

  return {
    animate: (ctx, w, h, mouse) => {
      rings.forEach((r) => {
        // Mouse: rings burst faster & bigger near cursor
        const dx = mouse.x - r.x;
        const dy = mouse.y - r.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        const force = dist < 200 ? (1 - dist / 200) * 2 : 0;

        r.radius += r.speed + force * 0.3;
        r.maxRadius = r.baseMax + force * 40;
        r.opacity = Math.min(
          0.4,
          0.15 * (1 - r.radius / r.maxRadius) + force * 0.1,
        );

        if (r.radius > r.maxRadius) {
          r.radius = 0;
          r.x = mouse.x + (Math.random() - 0.5) * 100;
          r.y = mouse.y + (Math.random() - 0.5) * 100;
          r.opacity = 0.15 + force * 0.05;
        }

        ctx.beginPath();
        ctx.arc(r.x, r.y, r.radius, 0, Math.PI * 2);
        ctx.strokeStyle = `rgba(${r.color}, ${r.opacity})`;
        ctx.lineWidth = 1.5 + force * 0.5;
        ctx.stroke();
        ctx.beginPath();
        ctx.arc(r.x, r.y, r.radius * 0.7, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${r.color}, ${r.opacity * 0.2})`;
        ctx.fill();
      });
    },
  };
};

gsap.ticker.lagSmoothing(0);

onMounted(() => {
  observeElements();
  startAutoPlay();

  // Init all section animations
  const hero = initSectionCanvas(canvasRef, initHero);
  if (hero) animCleanups.push(hero);

  const services = initSectionCanvas(servicesCanvasRef, initServices);
  if (services) animCleanups.push(services);

  const about = initSectionCanvas(aboutCanvasRef, initAbout);
  if (about) animCleanups.push(about);

  const why = initSectionCanvas(whyCanvasRef, initWhyChoose);
  if (why) animCleanups.push(why);

  const testimonial = initSectionCanvas(testimonialCanvasRef, initTestimonials);
  if (testimonial) animCleanups.push(testimonial);

  const faq = initSectionCanvas(faqCanvasRef, initFaq);
  if (faq) animCleanups.push(faq);

  const cta = initSectionCanvas(ctaCanvasRef, initCta);
  if (cta) animCleanups.push(cta);

  window.addEventListener("scroll", () => {
    const winScroll = document.documentElement.scrollTop;
    const height =
      document.documentElement.scrollHeight -
      document.documentElement.clientHeight;
    scrollProgress.value = (winScroll / height) * 100;
  });

  let hasRunOnce = false;

  const startTypedAnimation = () => {
    if (!typedHeadingRef.value) return;
    const heroEl = heroContentRef.value;
    if (!heroEl) return;

    // Destroy previous instance
    if (typedInstance) {
      typedInstance.destroy();
      typedInstance = null;
    }

    // Clear the heading content
    typedHeadingRef.value.innerHTML = "";

    const items = Array.from(heroEl.children);

    if (!hasRunOnce) {
      // First run: badge is already visible
      const badge = items[0];
      gsap.set(badge, { opacity: 1, y: 0 });
    }

    // Start Typed.js on heading
    typedInstance = new Typed(typedHeadingRef.value, {
      strings: [
        `Digital Solutions<br/><span class="gradient-text">For Tomorrow</span>`,
      ],
      typeSpeed: 50,
      startDelay: 200,
      backSpeed: 0,
      showCursor: true,
      cursorChar: "|",
      autoInsertCss: true,
      onComplete: () => {
        if (!hasRunOnce) {
          hasRunOnce = true;

          // First run only: animate paragraph, buttons & stats
          const para = items[1];
          const buttons = items[2];
          const statsEl = heroEl.querySelector(".hero-stats");

          gsap.fromTo(
            para,
            { opacity: 0, y: 30 },
            { opacity: 1, y: 0, duration: 0.7, ease: "power3.out" },
          );

          gsap.fromTo(
            buttons,
            { opacity: 0, y: 30, scale: 0.95 },
            {
              opacity: 1,
              y: 0,
              scale: 1,
              duration: 0.6,
              stagger: 0.1,
              ease: "back.out(1.7)",
              delay: 0.2,
            },
          );

          if (statsEl) {
            const statItems = statsEl.children;
            gsap.fromTo(
              statItems,
              { opacity: 0, y: 25 },
              {
                opacity: 1,
                y: 0,
                duration: 0.5,
                stagger: 0.06,
                delay: 0.4,
                ease: "power2.out",
              },
            );
          }
        }
      },
    });
  };

  // Hero entrance animation with Typed.js
  const heroEl = heroContentRef.value;
  if (heroEl) {
    const items = Array.from(heroEl.children);

    // 1. Badge fades in first
    const badge = items[0];
    gsap.fromTo(
      badge,
      { opacity: 0, y: -20 },
      { opacity: 1, y: 0, duration: 0.6, ease: "power2.out" },
    );

    // 2. Start Typed.js on heading after a short delay
    setTimeout(() => {
      startTypedAnimation();
    }, 400);
  }

  // 3. Re-trigger Typed.js every 10 seconds
  const TYPED_REFRESH_MS = 10000;
  setTimeout(() => {
    typedTimer = setInterval(startTypedAnimation, TYPED_REFRESH_MS);
  }, 6000); // Wait 6s for initial animation to settle
});

onBeforeUnmount(() => {
  stopAutoPlay();
  if (typedTimer) {
    clearInterval(typedTimer);
    typedTimer = null;
  }
  if (typedInstance) {
    typedInstance.destroy();
    typedInstance = null;
  }
  animCleanups.forEach(({ ticker, resizeHandler, mouseCleanup }) => {
    gsap.ticker.remove(ticker);
    window.removeEventListener("resize", resizeHandler);
    if (mouseCleanup) mouseCleanup();
  });
  animCleanups.length = 0;
  if (animationTween) {
    gsap.ticker.remove(animationTween);
    if (animationTween._resizeHandler) {
      window.removeEventListener("resize", animationTween._resizeHandler);
    }
  }
});
</script>

<template>
  <div class="relative">
    <!-- Scroll Progress Bar -->
    <div
      class="fixed top-20 left-0 h-1 bg-gradient-to-r from-primary via-secondary to-accent z-40 transition-all duration-150 shadow-lg shadow-primary/20"
      :style="{ width: scrollProgress + '%' }"
    ></div>
    <!-- Glow line beneath -->
    <div
      class="fixed top-[5.25rem] left-0 h-2 bg-gradient-to-r from-primary/20 via-secondary/10 to-accent/20 z-40 pointer-events-none"
      :style="{ width: scrollProgress + '%' }"
    ></div>

    <!-- ==================== HERO SECTION ==================== -->
    <section
      class="relative min-h-screen flex items-center justify-center overflow-hidden pt-20"
    >
      <!-- Background Effects -->
      <div class="absolute inset-0 gradient-hero"></div>
      <div class="absolute inset-0 bg-grid-pattern"></div>

      <!-- GSAP Particle Network -->
      <canvas
        ref="canvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.88"
      ></canvas>

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
        ref="heroContentRef"
        class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center"
      >
        <!-- Badge -->
        <div
          class="inline-flex items-center gap-2 px-3 sm:px-4 py-1.5 sm:py-2 rounded-full glass border border-primary/20 mb-4 sm:mb-6 md:mb-8"
        >
          <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
          <span class="text-sm text-gray-300"
            >Empowering Digital Innovation</span
          >
        </div>

        <!-- Main Heading -->
        <h1
          class="text-4xl sm:text-5xl md:text-7xl lg:text-8xl font-extrabold leading-tight mb-4 sm:mb-6 min-h-[1.2em] px-2"
        >
          <span ref="typedHeadingRef" class="typed-heading"></span>
        </h1>

        <p
          class="max-w-2xl mx-auto text-base sm:text-lg md:text-xl text-gray-400 mb-6 md:mb-10 px-2 sm:px-0"
        >
          We empower businesses with innovative technology solutions that drive
          growth, enhance efficiency, and create exceptional digital
          experiences.
        </p>

        <!-- CTA Buttons -->
        <div
          class="flex flex-col sm:flex-row items-center justify-center gap-3 sm:gap-4 w-full sm:w-auto px-4 sm:px-0"
        >
          <router-link
            to="/contact"
            class="group relative w-full sm:w-auto px-6 sm:px-8 py-3 sm:py-4 rounded-xl text-base sm:text-lg font-semibold text-white gradient-primary overflow-hidden transition-all duration-300 hover:shadow-2xl hover:shadow-primary/30 hover:scale-105 btn-border-glow"
          >
            <span class="relative z-10">Start Your Project</span>
            <div
              class="absolute inset-0 bg-white/10 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
            ></div>
          </router-link>

          <router-link
            to="/services"
            class="group relative w-full sm:w-auto px-6 sm:px-8 py-3 sm:py-4 rounded-xl text-base sm:text-lg font-semibold text-white glass border border-white/10 hover:border-primary/30 hover:bg-white/5 transition-all duration-300 btn-border-glow"
          >
            <span class="flex items-center justify-center gap-2">
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
          class="mt-10 sm:mt-14 md:mt-20 grid grid-cols-2 md:grid-cols-4 gap-4 sm:gap-6 md:gap-8 max-w-3xl mx-auto hero-stats"
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

    <!-- ==================== SERsectionECTION ==================== -->
    <section id="services" class="relative py-32 overflow-hidden">
      <!-- Background -->
      <div class="absolute inset-0 bg-darker"></div>
      <div
        class="absolute top-0 left-1/2 -translate-x-1/2 w-full h-px bg-gradient-to-r from-transparent via-primary/30 to-transparent"
      ></div>

      <!-- GSAP Floating Shapes -->
      <canvas
        ref="servicesCanvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.9"
      ></canvas>

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

      <!-- GSAP Data Waves -->
      <canvas
        ref="aboutCanvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.8"
      ></canvas>

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
              class="inline-flex items-center gap-2 mt-8 px-6 py-3 rounded-xl text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300 btn-border-glow"
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

      <!-- GSAP Rising Embers -->
      <canvas
        ref="whyCanvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.85"
      ></canvas>

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

      <!-- GSAP Orbiting Rings -->
      <canvas
        ref="testimonialCanvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.75"
      ></canvas>

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
    <!-- <section class="relative py-20 overflow-hidden bg-darker">
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
    </section> -->

    <!-- ==================== FAQ SECTION ==================== -->
    <section class="relative py-32 overflow-hidden">
      <div class="absolute inset-0 bg-dark"></div>

      <!-- GSAP Morphing Blobs -->
      <canvas
        ref="faqCanvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.75"
      ></canvas>

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
            class="inline-flex items-center gap-2 px-6 py-3 rounded-xl text-sm font-semibold text-white gradient-primary hover:shadow-lg hover:shadow-primary/30 transition-all duration-300 hover:scale-105 btn-border-glow"
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

      <!-- GSAP Pulsing Energy Rings -->
      <canvas
        ref="ctaCanvasRef"
        class="absolute inset-0 w-full h-full pointer-events-none"
        style="opacity: 0.8"
      ></canvas>

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
              class="group relative px-8 py-4 rounded-xl text-lg font-semibold text-white gradient-primary overflow-hidden transition-all duration-300 hover:shadow-2xl hover:shadow-primary/30 hover:scale-105 btn-border-glow"
            >
              <span class="relative z-10">Get In Touch</span>
              <div
                class="absolute inset-0 bg-white/10 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
              ></div>
            </router-link>
            <router-link
              to="/services"
              class="group px-8 py-4 rounded-xl text-lg font-semibold text-white glass border border-white/10 hover:border-primary/30 hover:bg-white/5 transition-all duration-300 btn-border-glow"
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

/* Typed.js cursor styling */
:deep(.typed-cursor) {
  color: #6366f1;
  font-weight: 100;
  animation: blink 0.8s infinite;
  font-size: 0.9em;
  opacity: 1;
}

@keyframes blink {
  0%,
  50% {
    opacity: 1;
  }
  51%,
  100% {
    opacity: 0;
  }
}
</style>
