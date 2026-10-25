<script setup>
import Nav from './components/Nav.vue';
import Technologies from './components/Technologies.vue';
import Carousel from './components/Carousel.vue';
import { siInstagram, siMinutemailer } from 'simple-icons';
import { ref, computed, watch } from 'vue';

const projects = [
  {
    name: 'Indeed Clone',
    category: 'Job Search Platform',
    description:
      'A job-search platform where users can explore opportunities, search listings, and view job details through a clean, responsive interface.',

    image: '/projects/indeed-clone.png',
    technologies: ['Laravel', 'PHP', 'PostgreSQL', 'Blade'],
    github: 'https://github.com/IlyasArbai/indeed-clone.git',

    // Additional screenshots shown inside the gallery modal.
    gallery: [
      '/projects/indeed-login.png',
      '/projects/indeed-register-type.png',
      '/projects/indeed-register-company.png',
      '/projects/indeed-register-user.png',
    ],
    live: '',
  },

  {
    name: 'Rituals Cosmetics Clone',
    category: 'E-commerce',
    description:
      'A cosmetics e-commerce storefront inspired by premium beauty brands, with a polished visual presentation and product-focused browsing experience.',

    image: '/projects/rituals-clone.png',
    technologies: ['Vue.js', 'Tailwind CSS', 'PostgreSQL', 'Express', 'Docker'], // Update with your actual stack
    github: 'https://github.com/IlyasArbai/rituals-clone.git', // Add your repository URL
    live: 'https://algorithmegenius.tech/', // Replace with your deployed URL

    gallery: [
      '/projects/rituals-products.png',
      '/projects/rituals-details.png',
      '/projects/rituals-dashboard.png',
      '/projects/rituals-login.png',
      '/projects/rituals-register.png',
    ],
  },

  // Add your next project using the same structure.
];

// Gallery modal state
const isGalleryOpen = ref(false);
const selectedProjectIndex = ref(0);
const selectedImageIndex = ref(0);

const selectedProject = computed(() => projects[selectedProjectIndex.value]);

const galleryImages = computed(() => {
  if (!selectedProject.value) return [];

  // The main screenshot is also available inside the gallery.
  return [selectedProject.value.image, ...selectedProject.value.gallery];
});

const currentImage = computed(() => galleryImages.value[selectedImageIndex.value]);

function openGallery(projectIndex, imageIndex = 0) {
  selectedProjectIndex.value = projectIndex;
  selectedImageIndex.value = imageIndex;
  resetZoom();
  isGalleryOpen.value = true;
}
function closeGallery() {
  isGalleryOpen.value = false;
  resetZoom();
}
function nextImage() {
  const total = galleryImages.value.length;

  if (total > 0) {
    selectedImageIndex.value = (selectedImageIndex.value + 1) % total;
  }
}
function previousImage() {
  const total = galleryImages.value.length;

  if (total > 0) {
    selectedImageIndex.value = (selectedImageIndex.value - 1 + total) % total;
  }
}

const zoom = ref(1);
const offset = ref({ x: 0, y: 0 });
let dragStart = null;

function resetZoom() {
  zoom.value = 1;
  offset.value = { x: 0, y: 0 };
  dragStart = null;
}

function changeZoom(amount) {
  zoom.value = Math.min(4, Math.max(1, zoom.value + amount));
  if (zoom.value === 1) resetZoom();
}

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

watch(currentImage, resetZoom);
</script>

<template>
  <Nav />

  <!-- About -->
  <section
    id="about"
    class="max-[375px]:h-full max-[375px]:scroll-mt-20 h-[75dvh] flex flex-col md:flex-row w-full items-center max-[375px]:py-5 py-10 px-10 md:px-20 xl:px-30 max-[375px]:gap-5 gap-10"
  >
    <!-- Hero Content -->
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

      <!-- Main Heading -->
      <h1
        class="max-w-3xl font-bold leading-none tracking-[-0.04em] text-white max-[375px]:text-[20px] text-[30px] sm:text-5xl xl:text-6xl"
      >
        I build with code.
        <br />
        <span class="text-neutral-500">I grow beyond it.</span>
      </h1>

      <!-- Description -->
      <p
        class="max-[375px]:mt-3 mt-6 max-w-xl max-[375px]:text-[8px] text-[12px] sm:text-base max-[375px]:leading-2.5 leading-5 text-neutral-400 sm:leading-6"
      >
        I’m a full-stack developer who enjoys turning ideas into useful digital experiences. But
        software is only one part of who I am. Exploring, learning, meeting people, and taking on
        new challenges shape the way I think, create, and solve problems.
      </p>

      <!-- Small statement -->
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
  <section class="border-t border-neutral-900">
    <Technologies />
  </section>

  <!-- Projects -->
  <section
    id="projects"
    class="flex min-h-screen flex-col bg-neutral-950 px-5 py-20 text-white sm:px-8 lg:h-[90vh] lg:min-h-0 lg:px-12 lg:py-8 xl:px-20"
  >
    <!-- Section heading -->
    <div class="mb-8 flex shrink-0 flex-col gap-3 sm:flex-row sm:items-end sm:justify-between">
      <div>
        <p class="mb-2 text-sm font-medium uppercase tracking-[0.25em] text-emerald-400">
          What I've built
        </p>

        <h2 class="text-3xl font-bold tracking-tight sm:text-4xl lg:text-5xl">
          Selected Projects<span class="text-emerald-400">.</span>
        </h2>
      </div>

      <p class="max-w-md text-sm leading-6 text-neutral-400">
        A selection of projects showcasing my development skills, technical decisions, and attention
        to user experience.
      </p>
    </div>

    <!-- Projects grid -->
    <div class="grid gap-6 sm:grid-cols-2 lg:min-h-0 lg:flex-1 lg:overflow-y-auto lg:pb-2">
      <article
        v-for="(project, index) in projects"
        :key="project.name"
        class="group flex min-w-0 flex-col overflow-hidden rounded-2xl border border-white/10 bg-neutral-900/70 transition duration-300 hover:-translate-y-1 hover:border-emerald-400/40 hover:bg-neutral-900"
      >
        <!-- Main project screenshot: no overlays -->
        <button
          type="button"
          class="block w-full overflow-hidden bg-neutral-800 text-left focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-inset focus-visible:ring-emerald-400"
          :aria-label="`Open ${project.name} gallery`"
          @click="openGallery(index)"
        >
          <img
            :src="project.image"
            :alt="`${project.name} main screenshot`"
            loading="lazy"
            class="aspect-[16/9] w-full object-cover object-top transition-transform duration-500 group-hover:scale-[1.02]"
          />
        </button>

        <!-- Project information -->
        <div class="flex flex-1 flex-col p-5 sm:p-6">
          <p class="mb-2 text-xs font-semibold uppercase tracking-[0.18em] text-emerald-400">
            {{ project.category }}
          </p>

          <h3 class="text-xl font-semibold tracking-tight">
            {{ project.name }}
          </h3>

          <p class="mt-3 text-sm leading-6 text-neutral-400">
            {{ project.description }}
          </p>

          <!-- Technology tags -->
          <div class="mt-4 flex flex-wrap gap-2">
            <span
              v-for="technology in project.technologies"
              :key="technology"
              class="rounded-full border border-white/10 bg-white/[0.03] px-3 py-1 text-xs text-neutral-300"
            >
              {{ technology }}
            </span>
          </div>

          <!-- Actions -->
          <div class="mt-6 flex flex-wrap items-center gap-3 border-t border-white/10 pt-4">
            <button
              v-if="project.gallery.length"
              type="button"
              class="inline-flex items-center gap-2 rounded-lg bg-emerald-400 px-4 py-2.5 text-sm font-semibold text-neutral-950 transition hover:bg-emerald-300 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-emerald-400 focus-visible:ring-offset-2 focus-visible:ring-offset-neutral-900"
              @click="openGallery(index)"
            >
              <span>View gallery</span>
              <span aria-hidden="true">↗</span>
            </button>

            <a
              v-if="project.github"
              :href="project.github"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center gap-2 rounded-lg border border-white/15 px-4 py-2.5 text-sm font-medium text-neutral-200 transition hover:border-white/40 hover:bg-white/5"
            >
              GitHub
              <span aria-hidden="true">↗</span>
            </a>

            <a
              v-if="project.live"
              :href="project.live"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center gap-2 rounded-lg border border-white/15 px-4 py-2.5 text-sm font-medium text-neutral-200 transition hover:border-white/40 hover:bg-white/5"
            >
              Live demo
              <span aria-hidden="true">↗</span>
            </a>
          </div>
        </div>
      </article>
    </div>

    <!-- Gallery modal -->
    <Teleport to="body">
      <div
        v-if="isGalleryOpen && selectedProject"
        class="fixed inset-0 z-[100] flex items-center justify-center bg-black/85 p-3 backdrop-blur-sm sm:p-6"
        role="dialog"
        aria-modal="true"
        :aria-label="`${selectedProject.name} screenshot gallery`"
        @click.self="closeGallery"
        @keydown.esc.window="closeGallery"
        @keydown.left.window="previousImage"
        @keydown.right.window="nextImage"
      >
        <div
          class="relative flex max-h-[95vh] w-full max-w-6xl flex-col overflow-hidden rounded-2xl border border-white/10 bg-neutral-950 shadow-2xl"
        >
          <!-- Modal header -->
          <div
            class="flex shrink-0 items-center justify-between gap-4 border-b border-white/10 px-4 py-4 sm:px-6"
          >
            <div class="min-w-0">
              <p class="text-xs font-medium uppercase tracking-[0.2em] text-emerald-400">
                {{ selectedProject.category }}
              </p>

              <h3 class="mt-1 truncate text-lg font-semibold text-white sm:text-xl">
                {{ selectedProject.name }}
              </h3>
            </div>

            <button
              type="button"
              class="flex size-10 shrink-0 items-center justify-center rounded-full border border-white/10 text-2xl text-neutral-300 transition hover:bg-white/10 hover:text-white"
              aria-label="Close gallery"
              @click="closeGallery"
            >
              &times;
            </button>
          </div>

          <!-- Main gallery image -->
          <div
            class="relative flex min-h-0 flex-1 items-center justify-center overflow-hidden bg-black p-3 sm:p-6"
            @wheel.prevent="changeZoom($event.deltaY < 0 ? 0.2 : -0.2)"
          >
            <img
              :key="currentImage"
              :src="currentImage"
              :alt="`${selectedProject.name} screenshot ${selectedImageIndex + 1}`"
              draggable="false"
              class="max-h-[55vh] max-w-full select-none object-contain sm:max-h-[60vh]"
              :class="zoom > 1 ? 'cursor-grab active:cursor-grabbing' : 'cursor-zoom-in'"
              :style="{
                transform: `translate(${offset.x}px, ${offset.y}px) scale(${zoom})`,
                transition: dragStart ? 'none' : 'transform 200ms ease',
                touchAction: 'none',
              }"
              @click="zoom === 1 && changeZoom(1)"
              @pointerdown="startDrag"
              @pointermove="dragImage"
              @pointerup="dragStart = null"
              @pointercancel="dragStart = null"
            />

            <!-- Zoom controls -->
            <div
              class="absolute bottom-5 left-1/2 z-20 flex -translate-x-1/2 items-center gap-3 rounded-full border border-white/10 bg-neutral-900/95 px-3 py-2 text-white shadow-lg"
            >
              <button
                type="button"
                aria-label="Zoom out"
                :disabled="zoom <= 1"
                class="px-2 text-xl disabled:opacity-30"
                @click="changeZoom(-0.5)"
              >
                −
              </button>

              <span class="min-w-12 text-center text-sm"> {{ Math.round(zoom * 100) }}% </span>

              <button
                type="button"
                aria-label="Zoom in"
                :disabled="zoom >= 4"
                class="px-2 text-xl disabled:opacity-30"
                @click="changeZoom(0.5)"
              >
                +
              </button>

              <button
                type="button"
                class="border-l border-white/20 pl-3 text-xs text-emerald-400"
                @click="resetZoom"
              >
                Reset
              </button>
            </div>

            <!-- Previous screenshot -->
            <button
              v-if="galleryImages.length > 1"
              type="button"
              aria-label="Previous screenshot"
              class="absolute left-3 top-1/2 z-10 flex size-10 -translate-y-1/2 items-center justify-center rounded-full bg-black/70 text-2xl text-white hover:bg-emerald-400 hover:text-black"
              @click="previousImage"
            >
              &#8249;
            </button>

            <!-- Next screenshot -->
            <button
              v-if="galleryImages.length > 1"
              type="button"
              aria-label="Next screenshot"
              class="absolute right-3 top-1/2 z-10 flex size-10 -translate-y-1/2 items-center justify-center rounded-full bg-black/70 text-2xl text-white hover:bg-emerald-400 hover:text-black"
              @click="nextImage"
            >
              &#8250;
            </button>
          </div>

          <!-- Thumbnail strip -->
          <div class="shrink-0 border-t border-white/10 px-4 py-4 sm:px-6">
            <div class="flex gap-3 overflow-x-auto pb-1">
              <button
                v-for="(image, imageIndex) in galleryImages"
                :key="image"
                type="button"
                class="w-24 shrink-0 overflow-hidden rounded-lg border-2 transition sm:w-32"
                :class="
                  selectedImageIndex === imageIndex
                    ? 'border-emerald-400'
                    : 'border-transparent opacity-60 hover:opacity-100'
                "
                :aria-label="`View screenshot ${imageIndex + 1}`"
                :aria-pressed="selectedImageIndex === imageIndex"
                @click="selectedImageIndex = imageIndex"
              >
                <img
                  :src="image"
                  :alt="`Screenshot thumbnail ${imageIndex + 1}`"
                  class="aspect-[16/10] w-full object-cover object-top"
                />
              </button>
            </div>

            <div class="mt-3 flex items-center justify-between gap-3 text-xs text-neutral-400">
              <span> Screenshot {{ selectedImageIndex + 1 }} of {{ galleryImages.length }} </span>

              <span class="hidden sm:inline"> Use ← → to navigate · Esc to close </span>
            </div>
          </div>
        </div>
      </div>
    </Teleport>
  </section>

  <!-- Contact -->
  <section
    id="contact"
    class="relative w-full overflow-hidden border-t border-neutral-900 px-10 md:px-20 xl:px-30 py-5 md:py-15 h-[75vh]"
  >
    <!-- Top label -->
    <div class="max-[375px]:mb-4 mb-8 flex items-center gap-3">
      <span class="h-px w-10 bg-emerald-400"></span>

      <p
        class="font-mono max-[375px]:text-[10px] text-xs font-medium uppercase tracking-[0.25em] text-emerald-400"
      >
        Get in touch
      </p>
    </div>

    <!-- Main content -->
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

      <!-- Contact actions -->
      <div class="flex flex-col gap-4">
        <!-- Email -->
        <a
          href="mailto:arbai.ilyas.dev@gmail.com"
          class="group inline-flex h-[58px] w-full items-center justify-between gap-8 rounded-2xl max-[375px]:h-[42px] max-[375px]:text-[10px] bg-emerald-500 px-6 py-4 text-sm font-semibold text-neutral-950 shadow-lg shadow-emerald-500/10 transition-all duration-300 hover:-translate-y-1 hover:bg-emerald-400 hover:shadow-emerald-500/20 whitespace-nowrap"
        >
          <span class="flex items-center gap-3">
            <svg
              :viewBox="siMinutemailer.svg.viewBox"
              class="h-6 w-6 shrink-0 text-black transition-colors"
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

        <!-- Instagram -->
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

    <!-- Bottom information -->
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
</template>
