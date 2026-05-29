<div align="center">
  <br/>
  <h1>SONTO</h1>
  <p><strong>Digital Solutions for Tomorrow</strong></p>
  <br/>
  <p>
    <img src="https://img.shields.io/badge/Vue_3-4FC08D?style=for-the-badge&logo=vue.js&logoColor=white" alt="Vue 3"/>
    <img src="https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white" alt="Vite"/>
    <img src="https://img.shields.io/badge/Tailwind_CSS_4-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS 4"/>
    <img src="https://img.shields.io/badge/GSAP-88CE02?style=for-the-badge&logo=greensock&logoColor=white" alt="GSAP"/>
    <img src="https://img.shields.io/badge/GitHub_Pages-222222?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Pages"/>
  </p>
  <br/>
</div>

## 📋 Overview

**SONTO** is a modern, interactive landing page for a digital solutions agency. Built with cutting-edge web technologies, it delivers a visually stunning experience through immersive canvas animations, glass morphism UI elements, and smooth page transitions.

### ✨ Key Features

- **Interactive Canvas Animations** — Particle networks, floating geometric shapes, data flow waves, orbiting rings, morphing blobs, and pulsing energy rings powered by GSAP
- **Typed.js Typing Effect** — Animated headline on the hero section
- **Scroll-Triggered Animations** — Elements fade in as they enter the viewport using Intersection Observer
- **Responsive Design** — Fully responsive layout across desktop, tablet, and mobile
- **Glass Morphism UI** — Modern frosted glass design system with backdrop blur effects
- **Animated Gradient Borders** — Conic gradient tracing borders on buttons and interactive elements
- **Smooth Page Transitions** — Out-in transitions between routes using Vue's `<Transition>` component
- **Dark Theme** — Elegant dark color scheme with custom gradients

## 🚀 Live Site

👉 [**ahmed-abdelgawad-dev.github.io/sonto-ds**](https://ahmed-abdelgawad-dev.github.io/sonto-ds/)

## 🧩 Pages

| Route | Page | Description |
|-------|------|-------------|
| `/` | **Home** | Hero with particle network, services overview, stats, testimonials carousel, FAQs, and CTA sections |
| `/services` | **Services** | Detailed service cards, process workflow, and consultation CTA |
| `/about` | **About** | Company story, milestones timeline, core values, and team members |
| `/contact` | **Contact** | Contact form, information channels, business hours, and phone CTA |

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| **Vue 3** | Frontend framework with Composition API & `<script setup>` |
| **Vite** | Build tool and development server |
| **Vue Router** | Client-side routing with scroll behavior management |
| **Tailwind CSS 4** | Utility-first CSS framework with custom theme |
| **GSAP** | High-performance canvas animations and timeline-based scrolling effects |
| **Typed.js** | Typewriter-style text animation for the hero heading |
| **@vueuse/core** | Vue composition utilities |

## 📁 Project Structure

```
src/
├── App.vue              # Root component (Navbar + Router View + Footer)
├── main.js              # Application entry point
├── style.css            # Tailwind imports, custom theme, glass/glow utilities
├── router/
│   └── index.js         # Route definitions and scroll behavior
├── components/
│   ├── Navbar.vue       # Responsive navigation with glass effect
│   ├── Footer.vue       # Footer with links and branding
│   └── WhatsAppButton.vue  # Floating WhatsApp contact button
└── views/
    ├── HomeView.vue     # Landing page (7 canvas animations)
    ├── ServicesView.vue # Services showcase
    ├── AboutView.vue    # Company information
    └── ContactView.vue  # Contact form and details
```

## 🚦 Getting Started

### Prerequisites

- **Node.js** >= 18
- **npm** >= 9

### Installation

```bash
# Clone the repository
git clone https://github.com/Ahmed-Abdelgawad-Dev/sonto-ds.git
cd sonto-ds

# Install dependencies
npm install

# Start the development server
npm run dev

# Build for production
npm run build

# Preview the production build
npm run preview
```

## 🌐 Deployment

The project is automatically deployed to **GitHub Pages** via GitHub Actions.

### Workflow

1. Push changes to the `main` branch.
2. GitHub Actions triggers the workflow defined in `.github/workflows/deploy.yml`.
3. The project is built and deployed to GitHub Pages.

### SPA Routing

The project includes client-side redirect logic for single-page application support on GitHub Pages, ensuring deep links (e.g., `/services`) resolve correctly.

## 🎨 Design System

### Colors

| Token | Value | Usage |
|-------|-------|-------|
| `primary` | `#6366f1` | Indigo — Primary actions, accents |
| `secondary` | `#06b6d4` | Cyan — Secondary elements, highlights |
| `accent` | `#f59e0b` | Amber — Accents, highlights |
| `dark` | `#0f172a` | Background |
| `darker` | `#020617` | Deeper background sections |

### Typography

- **Font:** Inter (Google Fonts)
- **Weights:** 300, 400, 500, 600, 700, 800

### UI Components

- **Glass morphism** — `background: rgba(255, 255, 255, 0.05)` with `backdrop-filter: blur(12px)`
- **Animated border glow** — Conic gradient tracing effect on buttons via CSS `@property` animation
- **Gradient text** — `linear-gradient(135deg, #6366f1, #06b6d4, #f59e0b)`
- **Grid pattern** — Subtle background grid overlay (`bg-grid-pattern`)

## 🧪 Canvas Animations

The home page features 7 distinct canvas-based interactive animations, each reacting to mouse movement:

| Section | Animation | Description |
|---------|-----------|-------------|
| Hero | **Particle Network** | 60 connected particles that attract to cursor |
| Services | **Floating Geometric Shapes** | Hexagons, triangles, and diamonds rotating in space |
| About | **Data Flow Sine Waves** | Animated sine waves with mouse-reactive amplitude |
| Why Choose Us | **Rising Ember Particles** | Glowing ember particles rising upward |
| Testimonials | **Orbiting Rings** | Planetary ring systems with orbiting dots |
| FAQ | **Fluid Morphing Blobs** | Organic blobs with deformable surfaces |
| CTA | **Pulsing Energy Rings** | Expanding ring bursts that follow the cursor |

## 📄 License

All rights reserved. This project is proprietary software.

---

<div align="center">
  <p>Crafted with ❤️ by the Sonto Web Team</p>
</div>
