<script setup>
import Nav from './components/Nav.vue';
import Technologies from './components/Technologies.vue';
import Carousel from './components/Carousel.vue';
import { siInstagram, siMinutemailer } from 'simple-icons';
import { ref, computed, watch, onMounted, onBeforeUnmount } from 'vue';

const projects = [
  {
    name: 'Messenger',
    category: 'Messaging & Real-Time Communication',
    description:
      'A real-time messaging platform that enables users to communicate seamlessly through conversations and manage chat requests. Built with a responsive interface and a robust backend to deliver a smooth and reliable messaging experience.',
    image: '/projects/messanger/conversation.png',
    technologies: [
      'React.js',
      'Tailwind CSS',
      'PostgreSQL',
      'Express.js',
      'Docker',
      'Socket.io',
      'Redis',
    ],
    github: 'https://github.com/IlyasArbai/Messanger',
    live: 'https://messanger.algorithmegenius.tech/',
    gallery: [
      '/projects/messanger/requests.png',
      '/projects/messanger/register.png',
      '/projects/messanger/login.png',
      '/projects/messanger/home.png',
    ],
  },

  {
    name: 'Tracker',
    category: 'Personal Finance & Expense Management',
    description:
      'A personal finance management application that enables users to track daily income and expenses, monitor monthly budgets, and manage their financial activities. Provides clear financial insights through transaction history, categorized spending, and monthly summaries to help users understand their spending habits and maintain better control over their finances.',
    image: '/projects/tracker/index.png',
    technologies: ['React Native', 'Nativewind', 'PostgreSQL', 'Express.js', 'Docker'],
    github: 'https://github.com/IlyasArbai/Tracker-app',
    server_github: 'https://github.com/IlyasArbai/Tracker-server',
    live: '',
    gallery: [
      '/projects/tracker/dashboard.jpeg',
      '/projects/tracker/daily.jpeg',
      '/projects/tracker/create.jpeg',
      '/projects/tracker/login.jpeg',
      '/projects/tracker/register.jpeg',
    ],
  },

  {
    name: 'Rituals Cosmetics Clone',
    category: 'E-commerce',
    description:
      'A cosmetics e-commerce storefront inspired by premium beauty brands, with a polished visual presentation and product-focused browsing experience.',
    image: '/projects/rituals/rituals-clone.png',
    technologies: ['Vue.js', 'Tailwind CSS', 'PostgreSQL', 'Express', 'Docker'],
    github: 'https://github.com/IlyasArbai/rituals-clone',
    live: 'https://algorithmegenius.tech/',
    gallery: [
      '/projects/rituals/rituals-products.png',
      '/projects/rituals/rituals-details.png',
      '/projects/rituals/rituals-dashboard.png',
      '/projects/rituals/rituals-login.png',
      '/projects/rituals/rituals-register.png',
    ],
  },

  {
    name: 'Indeed Clone',
    category: 'Job Search Platform',
    description:
      'A job-search platform where users can explore opportunities, search listings, and view job details through a clean, responsive interface.',
    image: '/projects/indeed/indeed-clone.png',
    technologies: ['Laravel', 'PHP', 'PostgreSQL', 'Blade'],
    github: 'https://github.com/IlyasArbai/indeed-clone.git',
    gallery: [
      '/projects/indeed/indeed-login.png',
      '/projects/indeed/indeed-register-type.png',
      '/projects/indeed/indeed-register-company.png',
      '/projects/indeed/indeed-register-user.png',
    ],
    live: '',
  },
];

// Gallery state
const isGalleryOpen = ref(false);
const selectedProjectIndex = ref(0);
const selectedImageIndex = ref(0);

const zoom = ref(1);
const offset = ref({ x: 0, y: 0 });

let dragStart = null;

const selectedProject = computed(() => projects[selectedProjectIndex.value]);

const galleryImages = computed(() => {
  if (!selectedProject.value) return [];

  return [selectedProject.value.image, ...selectedProject.value.gallery];
});

const currentImage = computed(() => galleryImages.value[selectedImageIndex.value]);

// Open gallery
function openGallery(projectIndex, imageIndex = 0) {
  selectedProjectIndex.value = projectIndex;
  selectedImageIndex.value = imageIndex;
  resetZoom();
  isGalleryOpen.value = true;
}

// Close gallery
function closeGallery() {
  isGalleryOpen.value = false;
  resetZoom();
}

// Image navigation
function nextImage() {
  const total = galleryImages.value.length;
  if (!total) return;

  selectedImageIndex.value = (selectedImageIndex.value + 1) % total;

  resetZoom();
}

function previousImage() {
  const total = galleryImages.value.length;
  if (!total) return;

  selectedImageIndex.value = (selectedImageIndex.value - 1 + total) % total;

  resetZoom();
}

function selectImage(index) {
  selectedImageIndex.value = index;
  resetZoom();
}

// Zoom
function resetZoom() {
  zoom.value = 1;
  offset.value = { x: 0, y: 0 };
  dragStart = null;
}

function changeZoom(amount) {
  zoom.value = Math.min(4, Math.max(1, zoom.value + amount));

  if (zoom.value === 1) {
    resetZoom();
  }
}

function toggleZoom() {
  if (zoom.value > 1) {
    resetZoom();
  } else {
    zoom.value = 2;
  }
}

function handleWheel(event) {
  changeZoom(event.deltaY < 0 ? 0.25 : -0.25);
}

// Image dragging
function startDrag(event) {
  if (zoom.value === 1) return;

  dragStart = {
    x: event.clientX - offset.value.x,
    y: event.clientY - offset.value.y,
  };

  event.currentTarget.setPointerCapture(event.pointerId);
}

function dragImage(event) {
  if (!dragStart) return;

  offset.value = {
    x: event.clientX - dragStart.x,
    y: event.clientY - dragStart.y,
  };
}

function endDrag() {
  dragStart = null;
}

// Keyboard controls
function handleKeydown(event) {
  if (!isGalleryOpen.value) return;

  if (event.key === 'Escape') closeGallery();

  if (event.key === 'ArrowRight') nextImage();

  if (event.key === 'ArrowLeft') previousImage();

  if (event.key === '+' || event.key === '=') {
    changeZoom(0.25);
  }

  if (event.key === '-') {
    changeZoom(-0.25);
  }

  if (event.key === '0') resetZoom();
}

// Reset zoom when image changes
watch(currentImage, resetZoom);

// Keyboard listener
onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown);
});
</script>

<template>
  <Nav />

  <!-- About -->
  <section
    id="about"
    class="max-[375px]:h-full h-[75dvh] flex flex-col md:flex-row w-full items-center max-[375px]:py-5 py-10 px-10 md:px-20 xl:px-30 max-[375px]:gap-5 gap-10"
  >
    <div class="w-full md:w-1/2">
      <div class="flex justify-between items-center w-full mb-5">
        <p
          class="max-[375px]:text-[7px] text-[10px] md:text-sm font-semibold uppercase tracking-[0.25em] text-neutral-500"
        >
          Ilyas Arbai
        </p>

        <div
          class="flex items-center gap-3 font-mono max-[375px]:text-[7px] text-[10px] md:text-sm font-medium uppercase tracking-[0.2em] text-emerald-400"
        >
          <span class="h-px w-8 bg-emerald-400"></span>
          Full-Stack Developer
        </div>
      </div>

      <h1
        class="max-w-3xl font-bold leading-none tracking-[-0.04em] text-white max-[375px]:text-[20px] text-[30px] sm:text-5xl xl:text-6xl"
      >
        I build with code.
        <br />
        <span class="text-neutral-500">I grow beyond it.</span>
      </h1>

      <p
        class="max-[375px]:mt-3 mt-6 max-w-xl max-[375px]:text-[8px] text-[12px] sm:text-base max-[375px]:leading-2.5 leading-5 text-neutral-400 sm:leading-6"
      >
        I’m a full-stack developer who enjoys turning ideas into useful digital experiences. But
        software is only one part of who I am. Exploring, learning, meeting people, and taking on
        new challenges shape the way I think, create, and solve problems.
      </p>

      <p
        class="max-[375px]:mt-3 mt-5 max-[375px]:text-[8px] text-[10px] font-medium text-neutral-300"
      >
        <span class="text-emerald-400">Code</span> is what I build with.
        <span class="text-neutral-500">Curiosity</span> is what drives me.
      </p>
    </div>

    <!-- Personal Carousel -->
    <div class="flex w-full justify-center md:w-1/2 md:justify-end">
      <Carousel />
    </div>
  </section>

  <!-- Technologies -->
  <section class="border-t border-neutral-900 mb-10">
    <Technologies />
  </section>

<!-- Projects -->
<section
  id="projects"
  class="relative flex h-[90dvh] w-full flex-col overflow-hidden py-4 sm:py-5"
>
  <!-- Section Header -->
  <div class="mb-3 flex shrink-0 items-end justify-between px-5 sm:mb-4 sm:px-10 md:px-20 xl:px-30">
    <div>
      <p
        class="mb-1.5 flex items-center gap-3 font-mono text-[10px] font-semibold uppercase tracking-[0.25em] text-emerald-400 sm:mb-2 sm:text-xs"
      >
        <span class="h-px w-8 bg-emerald-400"></span>
        Selected work
      </p>

      <h2 class="text-2xl font-bold tracking-tight text-white sm:text-4xl md:text-5xl">
        My Projects<span class="text-emerald-400">.</span>
      </h2>

      <p class="mt-1 text-[11px] text-neutral-500 sm:text-xs">
        A collection of things I've designed and built.
      </p>
    </div>

    <span class="hidden font-mono text-xs text-neutral-500 sm:block">
      {{ projects.length }} PROJECTS
    </span>
  </div>

  <!-- Projects Carousel -->
  <div
    class="flex min-h-0 flex-1 items-stretch gap-4 overflow-x-auto overflow-y-hidden px-5 pb-2 snap-x snap-mandatory scroll-smooth overscroll-x-contain sm:gap-6 sm:px-10 md:px-20 xl:px-30"
  >
    <article
      v-for="(project, index) in projects"
      :key="project.name"
      class="group relative flex h-full min-w-0 shrink-0 snap-start flex-col overflow-hidden rounded-2xl border border-white/[0.08] bg-neutral-900/70 transition-all duration-300 hover:border-emerald-400/30 hover:bg-neutral-900 basis-[88%] sm:basis-[65%] lg:basis-[48%] xl:basis-[40%]"
    >
      <!-- Screenshot -->
      <button
        type="button"
        class="relative block w-full shrink-0 overflow-hidden bg-neutral-800 text-left focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-inset focus-visible:ring-emerald-400"
        :aria-label="`Open ${project.name} gallery`"
        @click="openGallery(index)"
      >
        <img
          :src="project.image"
          :alt="`${project.name} main screenshot`"
          loading="lazy"
          class="aspect-[16/9] w-full object-cover object-top transition-transform duration-700 group-hover:scale-[1.04] lg:aspect-[2/1] xl:aspect-[16/7]"
        />

        <!-- Screenshot overlay -->
        <div
          class="absolute inset-0 flex items-center justify-center bg-black/0 transition duration-300 group-hover:bg-black/30"
        >
          <span
            class="translate-y-3 rounded-full border border-white/20 bg-neutral-950/80 px-5 py-2.5 text-xs font-semibold text-white opacity-0 backdrop-blur-md transition-all duration-300 group-hover:translate-y-0 group-hover:opacity-100"
          >
            View screenshots ↗
          </span>
        </div>

        <!-- Project number -->
        <span
          class="absolute left-4 top-4 rounded-lg border border-white/10 bg-neutral-950/70 px-3 py-1.5 font-mono text-[10px] text-neutral-300 backdrop-blur-md"
        >
          PROJECT {{ String(index + 1).padStart(2, '0') }}
        </span>
      </button>

      <!-- Project Information -->
      <div class="flex min-h-0 flex-1 flex-col p-4 sm:p-5 lg:p-6">

        <!-- Category, title and description -->
        <div class="shrink-0">
          <p
            class="mb-1.5 text-[10px] font-semibold uppercase tracking-[0.18em] text-emerald-400 sm:text-xs"
          >
            {{ project.category }}
          </p>

          <h3 class="text-lg font-bold tracking-tight text-white sm:text-xl lg:text-2xl">
            {{ project.name }}
          </h3>

          <p class="mt-2 line-clamp-2 text-xs leading-5 text-neutral-400 sm:text-sm sm:leading-6">
            {{ project.description }}
          </p>
        </div>

        <!-- Technologies -->
        <div class="mt-3 min-h-0 flex-1 overflow-hidden">
          <p class="mb-2 font-mono text-[9px] uppercase tracking-[0.18em] text-neutral-600">
            Built with
          </p>

          <div class="flex flex-wrap gap-1.5">
            <span
              v-for="technology in project.technologies"
              :key="technology"
              class="rounded-lg border border-white/[0.08] bg-white/[0.03] px-2.5 py-1.5 text-[10px] font-medium text-neutral-300 transition hover:border-emerald-400/30 hover:text-emerald-300 sm:text-xs"
            >
              {{ technology }}
            </span>
          </div>
        </div>

        <!-- Actions: Reserved bottom area -->
        <div
          class="mt-3 flex shrink-0 flex-wrap items-center gap-2 border-t border-white/[0.08] pt-3"
        >
          <!-- Gallery -->
          <button
            v-if="project.gallery?.length"
            type="button"
            @click="openGallery(index)"
            class="inline-flex items-center justify-center gap-2 rounded-xl bg-emerald-400 px-4 py-2.5 text-xs font-bold text-neutral-950 transition hover:bg-emerald-300 active:scale-95 sm:text-sm"
          >
            View gallery ↗
          </button>

          <!-- GitHub -->
          <a
            v-if="project.github"
            :href="project.github"
            target="_blank"
            rel="noopener noreferrer"
            class="inline-flex items-center justify-center gap-2 rounded-xl border border-white/10 px-4 py-2.5 text-xs font-semibold text-neutral-300 transition hover:border-white/30 hover:bg-white/5 hover:text-white sm:text-sm"
          >
            GitHub ↗
          </a>

          <!-- Server GitHub -->
          <a
            v-if="project.server_github"
            :href="project.server_github"
            target="_blank"
            rel="noopener noreferrer"
            class="inline-flex items-center justify-center gap-2 rounded-xl border border-white/10 px-4 py-2.5 text-xs font-semibold text-neutral-300 transition hover:border-white/30 hover:bg-white/5 hover:text-white sm:text-sm"
          >
            Server GitHub ↗
          </a>

          <!-- Live Demo -->
          <a
            v-if="project.live"
            :href="project.live"
            target="_blank"
            rel="noopener noreferrer"
            class="inline-flex items-center justify-center gap-2 rounded-xl border border-white/10 px-4 py-2.5 text-xs font-semibold text-neutral-300 transition hover:border-emerald-400/40 hover:bg-emerald-400/5 hover:text-emerald-300 sm:text-sm"
          >
            Live demo ↗
          </a>
        </div>
      </div>
    </article>
  </div>

  <!-- Carousel Hint -->
  <div class="mt-1 flex shrink-0 items-center justify-center gap-3 px-5">
    <span class="h-px w-8 bg-neutral-800"></span>
    <p class="font-mono text-[9px] uppercase tracking-[0.2em] text-neutral-600">
      Scroll to explore
    </p>
    <span class="h-px w-8 bg-neutral-800"></span>
  </div>
</section>

  <!-- Contact -->
  <section
    id="contact"
    class="relative w-full overflow-hidden border-t border-neutral-900 px-10 md:px-20 xl:px-30 py-5 md:py-15 h-[75vh]"
  >
    <div class="max-[375px]:mb-4 mb-8 flex items-center gap-3">
      <span class="h-px w-10 bg-emerald-400"></span>

      <p
        class="font-mono max-[375px]:text-[10px] text-xs font-medium uppercase tracking-[0.25em] text-emerald-400"
      >
        Get in touch
      </p>
    </div>

    <div class="flex flex-col max-[375px]:gap-5 gap-12 md:flex-row md:items-end md:justify-between">
      <div class="max-w-3xl">
        <h2
          class="max-[375px]:text-[20px] text-4xl font-bold leading-tight tracking-tight text-white md:text-5xl xl:text-6xl"
        >
          Have an idea?
          <br />
          <span class="text-neutral-500">Let's build it.</span>
        </h2>

        <p
          class="max-[375px]:mt-3 mt-5 text-[15px] leading-5 md:leading-7 text-neutral-400 md:text-lg max-w-xl max-[375px]:leading-2.5 max-[375px]:text-[8px]"
        >
          I'm open to new opportunities, freelance projects, collaborations, and interesting ideas.
          If you think we could build something useful together, I'd be happy to hear from you.
        </p>
      </div>

      <!-- Contact Actions -->
      <div class="flex flex-col gap-4">
        <a
          href="mailto:arbai.ilyas.dev@gmail.com"
          class="group inline-flex h-[58px] w-full items-center justify-between gap-8 rounded-2xl max-[375px]:h-[42px] max-[375px]:text-[10px] bg-emerald-500 px-6 py-4 text-sm font-semibold text-neutral-950 shadow-lg shadow-emerald-500/10 transition-all duration-300 hover:-translate-y-1 hover:bg-emerald-400 hover:shadow-emerald-500/20 whitespace-nowrap"
        >
          <span class="flex items-center gap-3">
            <svg
              :viewBox="siMinutemailer.svg.viewBox"
              class="h-6 w-6 shrink-0 text-black"
              fill="currentColor"
              aria-hidden="true"
            >
              <path :d="siMinutemailer.path" />
            </svg>

            <span>Send me an email</span>
          </span>

          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="h-5 w-5 shrink-0 transition-transform duration-300 group-hover:translate-x-1"
          >
            <path
              d="M3.5 10a.75.75 0 0 1 .75-.75h9.69L10.22 5.53a.75.75 0 0 1 1.06-1.06l5 5a.75.75 0 0 1 0 1.06l-5 5a.75.75 0 1 1-1.06-1.06l3.72-3.72H4.25A.75.75 0 0 1 3.5 10Z"
            />
          </svg>
        </a>

        <a
          href="https://instagram.com/ilyas_arbai1"
          target="_blank"
          rel="noopener noreferrer"
          class="group inline-flex h-[58px] w-full items-center justify-between gap-8 rounded-2xl border border-neutral-800 bg-neutral-950/50 px-6 py-4 text-sm font-medium text-neutral-300 transition-all duration-300 hover:-translate-y-1 hover:border-pink-500/40 hover:bg-pink-500/5 hover:text-white max-[375px]:h-[42px] max-[375px]:text-[10px]"
        >
          <span class="flex items-center gap-3">
            <svg
              :viewBox="siInstagram.svg.viewBox"
              class="h-6 w-6 shrink-0 text-neutral-400 transition-colors group-hover:text-pink-500"
              fill="currentColor"
              aria-hidden="true"
            >
              <path :d="siInstagram.path" />
            </svg>

            <span>Instagram</span>
          </span>

          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="h-5 w-5 shrink-0 text-neutral-600 transition-all duration-300 group-hover:translate-x-1 group-hover:text-neutral-300"
          >
            <path
              d="M5.22 14.78a.75.75 0 0 0 1.06 0L14 7.06v5.69a.75.75 0 0 0 1.5 0V5.25a.75.75 0 0 0-.75-.75H6.25a.75.75 0 0 0 0 1.5h5.69l-6.72 6.72a.75.75 0 0 0 0 1.06Z"
            />
          </svg>
        </a>
      </div>
    </div>

    <!-- Bottom Information -->
    <div
      class="mt-10 flex flex-col md:flex-row items-center justify-between border-t border-neutral-900 pt-6"
    >
      <div class="flex items-center gap-3">
        <span
          class="h-2 w-2 rounded-full bg-emerald-400 shadow-[0_0_10px_rgba(52,211,153,0.6)]"
        ></span>

        <p class="text-sm text-neutral-400">Available for new opportunities</p>
      </div>

      <p class="font-mono text-xs text-neutral-600">Let's make something meaningful.</p>
    </div>
  </section>

  <!-- Footer -->
  <footer
    class="w-full border-t border-neutral-900 px-10 md:px-20 xl:px-30 h-[15vh] flex flex-row justify-between items-center text-sm text-neutral-600 text-center"
  >
    <p>
      © {{ new Date().getFullYear() }}
      <span class="text-neutral-400">Ilyas Arbai</span>
    </p>

    <p class="font-mono text-xs">
      Built with
      <span class="text-emerald-400">Vue.js</span>
    </p>
  </footer>

  <!-- Full-screen Project Gallery -->
  <Teleport to="body">
    <Transition name="gallery">
      <div
        v-if="isGalleryOpen"
        class="fixed inset-0 z-[100] flex flex-col bg-neutral-950/95 backdrop-blur-xl"
        @click.self="closeGallery"
      >
        <!-- Gallery Header -->
        <header
          class="flex shrink-0 items-center justify-between border-b border-white/10 px-5 py-4 sm:px-8"
        >
          <div>
            <h3 class="text-sm font-semibold text-white sm:text-base">
              {{ selectedProject.name }}
            </h3>

            <p class="mt-1 text-xs text-neutral-500">
              {{ selectedProject.category }}
            </p>
          </div>

          <div class="flex items-center gap-4">
            <span class="text-xs tabular-nums text-neutral-400">
              {{ selectedImageIndex + 1 }} / {{ galleryImages.length }}
            </span>

            <button
              type="button"
              @click="closeGallery"
              aria-label="Close gallery"
              class="flex h-10 w-10 items-center justify-center rounded-full border border-white/10 text-white transition hover:bg-white/10"
            >
              <span class="text-xl">✕</span>
            </button>
          </div>
        </header>

        <!-- Main Viewer -->
        <main
          class="relative flex min-h-0 flex-1 items-center justify-center overflow-hidden px-14 py-4 sm:px-20"
          @wheel.prevent="handleWheel"
        >
          <!-- Previous Button -->
          <button
            type="button"
            @click="previousImage"
            aria-label="Previous image"
            class="absolute left-3 z-20 flex h-10 w-10 items-center justify-center rounded-full border border-white/10 bg-neutral-900/80 text-2xl text-white transition hover:border-emerald-400 hover:bg-emerald-400 hover:text-black sm:left-8"
          >
            ‹
          </button>

          <!-- Current Image -->
          <img
            :key="currentImage"
            :src="currentImage"
            :alt="`${selectedProject.name} screenshot ${selectedImageIndex + 1}`"
            draggable="false"
            @dblclick="toggleZoom"
            @pointerdown="startDrag"
            @pointermove="dragImage"
            @pointerup="endDrag"
            @pointercancel="endDrag"
            :style="{
              transform: `translate(${offset.x}px, ${offset.y}px) scale(${zoom})`,
              cursor: zoom > 1 ? 'grab' : 'zoom-in',
              touchAction: 'none',
            }"
            class="max-h-full max-w-full select-none rounded-lg object-contain transition-transform duration-150"
          />

          <!-- Next Button -->
          <button
            type="button"
            @click="nextImage"
            aria-label="Next image"
            class="absolute right-3 z-20 flex h-10 w-10 items-center justify-center rounded-full border border-white/10 bg-neutral-900/80 text-2xl text-white transition hover:border-emerald-400 hover:bg-emerald-400 hover:text-black sm:right-8"
          >
            ›
          </button>
        </main>

        <!-- Zoom Controls -->
        <div class="flex shrink-0 items-center justify-center gap-3 py-3">
          <button
            type="button"
            @click="changeZoom(-0.5)"
            aria-label="Zoom out"
            class="flex h-9 w-10 items-center justify-center rounded-lg border border-white/10 text-lg text-white transition hover:bg-white/10"
          >
            −
          </button>

          <button
            type="button"
            @click="resetZoom"
            class="min-w-16 rounded-lg border border-white/10 px-3 py-2 text-xs tabular-nums text-neutral-300 transition hover:bg-white/10"
          >
            {{ Math.round(zoom * 100) }}%
          </button>

          <button
            type="button"
            @click="changeZoom(0.5)"
            aria-label="Zoom in"
            class="flex h-9 w-10 items-center justify-center rounded-lg border border-white/10 text-lg text-white transition hover:bg-white/10"
          >
            +
          </button>
        </div>

        <!-- Thumbnail Navigation -->
        <div
          class="flex shrink-0 justify-center gap-3 overflow-x-auto border-t border-white/10 px-4 py-4"
        >
          <button
            v-for="(image, index) in galleryImages"
            :key="image"
            type="button"
            @click="selectImage(index)"
            :aria-label="`View image ${index + 1}`"
            :aria-current="selectedImageIndex === index ? 'true' : undefined"
            class="h-14 w-20 shrink-0 overflow-hidden rounded-lg border-2 transition sm:h-16 sm:w-24"
            :class="
              selectedImageIndex === index
                ? 'border-emerald-400 opacity-100'
                : 'border-transparent opacity-50 hover:opacity-100'
            "
          >
            <img
              :src="image"
              :alt="`Thumbnail ${index + 1}`"
              loading="lazy"
              class="h-full w-full object-cover"
            />
          </button>
        </div>

        <!-- Keyboard Shortcuts -->
        <p class="shrink-0 pb-3 text-center text-[10px] text-neutral-600">
          ← → Navigate · + − Zoom · 0 Reset · Double-click to zoom · ESC Close
        </p>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.gallery-enter-active,
.gallery-leave-active {
  transition: opacity 0.2s ease;
}

.gallery-enter-from,
.gallery-leave-to {
  opacity: 0;
}
</style>
