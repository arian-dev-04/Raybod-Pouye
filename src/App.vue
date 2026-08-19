<template>
  <div
    class="app"
    :class="{
      'is-rtl': currentLanguage === 'fa',
      'is-ltr': currentLanguage === 'en',
      'is-switching': isSwitchingLanguage,
    }"
    :dir="currentLanguage === 'fa' ? 'rtl' : 'ltr'"
  >
    <Transition name="ray-loader">
      <div v-if="isInitialLoading" class="ray-loader" aria-hidden="true">
        <div class="ray-loader__pieces">
          <span class="ray-piece ray-piece--1"></span>
          <span class="ray-piece ray-piece--2"></span>
          <span class="ray-piece ray-piece--3"></span>
          <span class="ray-piece ray-piece--4"></span>
          <span class="ray-piece ray-piece--5"></span>
          <span class="ray-piece ray-piece--6"></span>
          <span class="ray-piece ray-piece--7"></span>
          <span class="ray-piece ray-piece--8"></span>
          <span class="ray-piece ray-piece--9"></span>
        </div>

        <div class="ray-loader__center">
          <div class="ray-loader__mark"><span></span></div>
          <div class="ray-loader__brand">{{ t("BrandName") }}</div>
        </div>

        <div class="ray-loader__flash"></div>
      </div>
    </Transition>

    <!-- ================= Header ================= -->
    <header class="header" :class="{ scrolled: isScrolled }">
      <div class="container header__inner">
        <a href="#home" class="brand">
          <span class="brand__mark"></span>
          <span class="brand__text">{{ t("BrandName") }}</span>
        </a>

        <nav class="nav" :class="{ active: isMenuOpen }">
          <a
            v-for="item in navItems"
            :key="item.href"
            :href="item.href"
            :class="{ 'is-active': activeSection === item.href.slice(1) }"
            @click="isMenuOpen = false"
          >
            {{ t(item.label) }}
          </a>

          <button
            class="lang-btn"
            type="button"
            :aria-label="
              currentLanguage === 'en'
                ? 'Switch to Persian'
                : 'Switch to English'
            "
            @click="toggleLanguage"
          >
            <span>{{ currentLanguage === "en" ? "فا" : "En" }}</span>
          </button>

          <!-- Language switch inside mobile/tablet menu -->
          <button
            class="nav-language"
            type="button"
            :aria-label="
              currentLanguage === 'en'
                ? 'Switch to Persian'
                : 'Switch to English'
            "
            @click="toggleLanguage"
          >
            <span>{{ currentLanguage === "en" ? "فا" : "En" }}</span>
            <span>{{ currentLanguage === "en" ? "فارسی" : "English" }}</span>
          </button>
        </nav>

        <button
          class="menu-btn"
          type="button"
          aria-label="Menu"
          :class="{ active: isMenuOpen }"
          @click="isMenuOpen = !isMenuOpen"
        >
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>
    </header>

    <!-- ================= Hero ================= -->
    <section id="home" class="hero section">
      <div class="hero__image">
        <img :src="getImage('hero-1.jpg')" alt="Hero" />
      </div>

      <div class="container hero__content">
        <div class="hero__text reveal reveal--visible">
          <h1>
            {{ t("Create") }}<br />
            {{ t("Business Solution") }}
          </h1>

          <p>
            {{ t("Lorem ipsum dolor sit amet consectetur adipisicing") }}
          </p>

          <a href="#contact" class="btn btn--primary">
            {{ t("WRITE TO US") }}
            <span>></span>
          </a>
        </div>
      </div>

      <div class="hero__scroll-cue" aria-hidden="true">
        <span></span>
      </div>
    </section>

    <!-- ================= About ================= -->
    <section id="about" class="about section">
      <div class="container about__inner">
        <div class="about__visual reveal reveal--left" v-reveal>
          <div class="soft-diamond soft-diamond--one"></div>
          <div class="soft-diamond soft-diamond--two"></div>

          <div class="about-card">
            <div class="diamond-title">
              <span>
                {{ t("About") }}<br />
                {{ t("Us") }}
              </span>
            </div>

            <div class="about-image diamond-image">
              <img
                src="https://images.unsplash.com/photo-1494526585095-c41746248156?q=80&w=800&auto=format&fit=crop"
                alt="Office"
              />
            </div>
          </div>
        </div>

        <div class="about__content reveal reveal--right" v-reveal>
          <h2>
            {{ t("Nulla lobortis nunc vitae nisi semper semper velit") }}
          </h2>

          <div class="stats">
            <div class="stat">
              <strong>
                {{ animatedStats.employee }}
                <span class="stat__plus">+</span>
              </strong>
              <span>{{ t("Employee") }}</span>
            </div>

            <div class="stat">
              <strong>
                {{ animatedStats.projects }}
                <span class="stat__plus">+</span>
              </strong>
              <span>{{ t("Projects") }}</span>
            </div>

            <div class="stat">
              <strong>
                {{ animatedStats.clients }}
                <span class="stat__plus">+</span>
              </strong>
              <span>{{ t("Clients") }}</span>
            </div>
          </div>

          <blockquote>
            {{
              t(
                "Aliquam lobortis magna neque, gravida consequat velit venenatis at.",
              )
            }}
          </blockquote>
        </div>
      </div>
    </section>

    <!-- ================= Services ================= -->
    <section id="services" class="services section">
      <!-- Floating Title -->
      <div class="services__floating-title">
        <div class="container">
          <div class="services__title-wrapper">
            <div class="services__title-icon">
              <span class="services__icon">◆</span>
              <h2>
                {{ t("Our") }}<br />
                {{ t("Services") }}
              </h2>
            </div>
            <nav class="services__arrows">
              <button
                type="button"
                class="services__arrow-btn services__arrow-btn--prev"
                aria-label="Previous"
                @click="goToPreviousService"
              >
                ‹
              </button>
              <button
                type="button"
                class="services__arrow-btn services__arrow-btn--next"
                aria-label="Next"
                @click="goToNextService"
              >
                ›
              </button>
            </nav>
          </div>
        </div>
      </div>

      <!-- Slider -->
      <div
        ref="servicesCards"
        class="services__slider"
        @pointerdown="startServicesDrag"
        @pointermove="moveServicesDrag"
        @pointerup="endServicesDrag"
        @pointercancel="endServicesDrag"
        @pointerleave="endServicesDrag"
        @mouseenter="pauseServicesAutoSlide"
        @mouseleave="resumeServicesAutoSlide"
      >
        <div ref="servicesTrack" class="services__track">
          <div class="services__spacer" aria-hidden="true"></div>

          <article
            v-for="(service, index) in servicesList"
            :key="`${service.img}-${index}`"
            class="service-card"
          >
            <div class="service-card__media">
              <img
                :src="service.img"
                :alt="t(service.title)"
                draggable="false"
              />
            </div>
            <div class="service-card__body">
              <h3>{{ t(service.title) }}</h3>
              <p>{{ t(service.desc) }}</p>
              <a href="#contact" class="service-card__btn">
                {{ t("SEE DETAIL") }}
                <span class="service-card__arrow">›</span>
              </a>
            </div>
          </article>

          <div class="services__spacer" aria-hidden="true"></div>
        </div>
      </div>
    </section>

    <!-- ================= Expertise ================= -->
    <section id="expertise" class="expertise section">
      <div class="container expertise__inner">
        <div class="expertise__visual reveal reveal--left" v-reveal>
          <div class="expertise-orbit">
            <div class="orbit-line orbit-line--one"></div>
            <div class="orbit-line orbit-line--two"></div>

            <div class="expertise-title diamond-title diamond-title--large">
              <span>
                {{ t("Our") }}<br />
                {{ t("Expertise") }}
              </span>
            </div>

            <div class="icon-bubble icon-bubble--camera">📷</div>
            <div class="icon-bubble icon-bubble--play">▶</div>
            <div class="icon-bubble icon-bubble--wifi">☊</div>
            <div class="icon-bubble icon-bubble--star">★</div>
            <div class="icon-bubble icon-bubble--lab">♟</div>
            <div class="icon-bubble icon-bubble--small">✹</div>
          </div>
        </div>

        <div class="expertise__responsive-title">
          {{ t("Our Expertise") }}
        </div>

        <div class="expertise__content reveal reveal--right" v-reveal>
          <h2>
            {{ t("Nulla lobortis nunc vitae nisi semper semper velit") }}
          </h2>

          <p>
            {{
              t(
                "Curabitur egestas consequat lorem, vel fermentum augue porta id. Aliquam lobortis magna neque, gravida consequat velit venenatis at. Duis sed augue.",
              )
            }}
          </p>

          <div class="tags">
            <span>{{ t("Marketing") }}</span>
            <span>{{ t("SEO") }}</span>
            <span>{{ t("Social Media") }}</span>
            <span class="active">{{ t("Web Development") }}</span>
            <span class="active">{{ t("UI Design") }}</span>
            <span class="active">{{ t("Mobile Apps") }}</span>
            <span>{{ t("Photography") }}</span>
            <span>{{ t("Company Profile") }}</span>
            <span>{{ t("Visual Editing") }}</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ================= Testimonials ================= -->
    <section id="testimonials" class="testimonials section">
      <div class="container testimonials__inner">
        <div class="testimonials__title reveal reveal--left" v-reveal>
          <div class="testimonial-diamond">
            <div class="testimonial-quote">“</div>

            <h2>
              {{ t("Client") }}<br />
              {{ t("Testimonials") }}
            </h2>
          </div>
        </div>

        <div
          ref="testimonialsCards"
          class="testimonials__cards reveal reveal--right"
          v-reveal
          @pointerdown="startTestimonialsDrag"
          @pointermove="moveTestimonialsDrag"
          @pointerup="endTestimonialsDrag"
          @pointercancel="endTestimonialsDrag"
          @pointerleave="endTestimonialsDrag"
          @mouseenter="pauseTestimonialsAutoSlide"
          @mouseleave="resumeTestimonialsAutoSlide"
        >
          <div class="testimonials__track">
            <article
              v-for="(item, index) in displayedTestimonials"
              :key="`${item.name}-${item.role}-${index}`"
              class="testimonial-card"
            >
              <div class="testimonial-card__box">
                <div
                  class="testimonial-card__text-wrap"
                  :class="{
                    'testimonial-card__text-wrap--rtl':
                      currentLanguage === 'fa',
                  }"
                >
                  <p class="testimonial-card__text">
                    {{ t(item.text) }}
                  </p>
                </div>

                <div
                  class="testimonial-card__stars"
                  :aria-label="`${item.rating} out of 5 stars`"
                >
                  <span
                    v-for="star in 5"
                    :key="star"
                    :class="{ 'is-empty': star > item.rating }"
                  >
                    ★
                  </span>
                </div>
              </div>

              <div class="testimonial-card__client">
                <img
                  class="testimonial-card__avatar"
                  :src="item.image"
                  :alt="item.name"
                  draggable="false"
                />

                <div class="testimonial-card__info">
                  <h3>{{ item.name }}</h3>
                  <span>{{ t(item.role) }}</span>
                </div>
              </div>
            </article>
          </div>
        </div>

        <div class="testimonial-dots" aria-label="Testimonial slider dots">
          <button
            v-for="(_, index) in testimonials"
            :key="index"
            type="button"
            :class="{ active: activeTestimonialDot === index }"
            :aria-label="`Slide ${index + 1}`"
            @click="goToTestimonial(index)"
          ></button>
        </div>
      </div>
    </section>

    <!-- ================= Case Studies ================= -->
    <section id="case-studies" class="case section">
      <div class="container case__inner">
        <aside class="case__sidebar reveal reveal--left" v-reveal>
          <h2>
            {{ t("Case") }}<br />
            {{ t("Studies") }}
          </h2>

          <ul>
            <li class="active">{{ t("Corporate") }}</li>
            <li>{{ t("Advertising") }}</li>
            <li>{{ t("Marketing") }}</li>
            <li>{{ t("Government") }}</li>
            <li>{{ t("Creative") }}</li>
          </ul>
        </aside>

        <div class="case__grid reveal reveal--right" v-reveal>
          <article
            v-for="(project, index) in projects"
            :key="`${project.title}-${index}`"
            class="case-card"
            :class="project.class"
          >
            <img
              v-if="project.image"
              :src="project.image"
              :alt="t(project.title)"
            />

            <div v-if="project.logo" class="case-logo">RP</div>

            <div class="case-card__overlay">
              <span v-if="index !== 1" class="case-icon">◆</span>

              <h3>{{ t(project.title) }}</h3>

              <p>{{ t("Vestibulum consequat hendrerit.") }}</p>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- ================= CTA ================= -->
    <section class="cta">
      <div class="container reveal reveal--up" v-reveal>
        <div class="cta__box">
          <div>
            <h3>{{ t("Ready to get started ?") }}</h3>

            <p>
              {{
                t("Pellentesque ac bibendum tortor. Nulla eget lobortis lacus.")
              }}
            </p>
          </div>

          <a href="#contact" class="btn btn--primary">
            {{ t("CONTACT US") }}
            <span>></span>
          </a>
        </div>
      </div>
    </section>

    <!-- ================= Contact / Office ================= -->
    <section id="contact" class="office section">
      <div class="container office__inner">
        <div class="office__left reveal reveal--left" v-reveal>
          <div class="diamond-title office-title">
            <span>
              {{ t("Our") }}<br />
              {{ t("Office") }}
            </span>
          </div>

          <div class="contact-card">
            <h4>{{ t("Head Quarter") }}</h4>

            <div class="contact-row">
              <span>☎ {{ t("+123 456 789 01") }}</span>
              <span>✉ {{ t("hello@lulu.com") }}</span>
            </div>

            <p>📍 {{ t("Lorem ipsum street no 14 Block A") }}</p>
          </div>

          <div class="contact-card">
            <h4>{{ t("Branch Office") }}</h4>

            <div class="contact-row">
              <span>☎ {{ t("+98 765 432 10") }}</span>
              <span>✉ {{ t("hello@lulu.com") }}</span>
            </div>

            <p>
              📍
              {{
                t("Vivamus street Block C - Vestibulum Building - 3rd Floor")
              }}
            </p>
          </div>
        </div>

        <div class="map reveal reveal--right" v-reveal>
          <img
            src="https://images.unsplash.com/photo-1524661135-423995f22d0b?q=80&w=1000&auto=format&fit=crop"
            alt="Map"
          />

          <div class="map-pin"></div>

          <button class="map-btn" type="button">
            {{ t("Services ◆") }}
          </button>
        </div>
      </div>
    </section>

    <!-- ================= Footer ================= -->
    <footer class="footer">
      <div class="container footer__inner">
        <!-- Brand -->
        <div class="footer__brand">
          <a href="#home" class="brand brand--footer">
            <span class="brand__mark"></span>
            <span class="brand__text">{{ t("BrandName") }}</span>
          </a>

          <p>
            {{
              t(
                "Nam posuere accumsan porta. Integer id orci sed ante tincidunt tincidunt at sed libero.",
              )
            }}
          </p>

          <small>{{ t("© Lu Theme 2019") }}</small>
        </div>

        <!-- Company -->
        <div
          class="footer-col"
          :class="{
            'is-open': openFooterSection === 'company',
          }"
        >
          <button
            class="footer-col__toggle"
            type="button"
            :aria-expanded="openFooterSection === 'company'"
            @click="toggleFooterSection('company')"
          >
            <span>{{ t("COMPANY") }}</span>
            <span class="footer-col__arrow">⌄</span>
          </button>

          <div class="footer-col__links">
            <a href="#about">{{ t("Donec dignissim") }}</a>
            <a href="#services">{{ t("Curabitur egestas") }}</a>
            <a href="#expertise">{{ t("Nam posuere") }}</a>
            <a href="#case-studies">{{ t("Aenean facilisis") }}</a>
          </div>
        </div>

        <!-- Services -->
        <div
          class="footer-col"
          :class="{
            'is-open': openFooterSection === 'services',
          }"
        >
          <button
            class="footer-col__toggle"
            type="button"
            :aria-expanded="openFooterSection === 'services'"
            @click="toggleFooterSection('services')"
          >
            <span>{{ t("SERVICES") }}</span>
            <span class="footer-col__arrow">⌄</span>
          </button>

          <div class="footer-col__links">
            <a href="#services">{{ t("Cras convallis") }}</a>
            <a href="#services">{{ t("Vestibulum faucibus") }}</a>
            <a href="#services">{{ t("Quisque lacinia purus") }}</a>
            <a href="#contact">{{ t("Aliquam nec ex") }}</a>
          </div>
        </div>

        <!-- Resources -->
        <div
          class="footer-col"
          :class="{
            'is-open': openFooterSection === 'resources',
          }"
        >
          <button
            class="footer-col__toggle"
            type="button"
            :aria-expanded="openFooterSection === 'resources'"
            @click="toggleFooterSection('resources')"
          >
            <span>{{ t("RESOURCES") }}</span>
            <span class="footer-col__arrow">⌄</span>
          </button>

          <div class="footer-col__links">
            <a href="#contact">{{ t("Suspendisse porttitor") }}</a>
            <a href="#expertise">{{ t("Nam posuere") }}</a>
            <a href="#services">{{ t("Curabitur egestas") }}</a>
          </div>
        </div>

        <!-- Social -->
        <div class="footer-social">
          <div class="socials">
            <a href="#" aria-label="Facebook">f</a>
            <a href="#" aria-label="LinkedIn">in</a>
            <a href="#" aria-label="X">x</a>
            <a href="#" aria-label="Instagram">ig</a>
          </div>

          <select aria-label="Language">
            <option>{{ t("English - En") }}</option>
            <option>{{ t("Persian - Fa") }}</option>
          </select>
        </div>
      </div>
    </footer>

    <button
      class="to-top"
      type="button"
      aria-label="Back to top"
      :class="{ show: isScrolled }"
      @click="scrollTop"
    >
      ↑
    </button>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from "vue";

const images = import.meta.glob("./assets/images/*", {
  eager: true,
  import: "default",
});

const getImage = (name) => {
  return images[`./assets/images/${name}`];
};

/* =========================================================
   STATE
   ========================================================= */

const isScrolled = ref(false);
const isMenuOpen = ref(false);
const isInitialLoading = ref(true);
const currentLanguage = ref("en");
const activeSection = ref("home");
const isSwitchingLanguage = ref(false);

/* =========================================================
   FOOTER ACCORDION
   فقط برای ریسپانسیو فوتر
   ========================================================= */

const openFooterSection = ref(null);

const toggleFooterSection = (section) => {
  openFooterSection.value =
    openFooterSection.value === section ? null : section;
};

/* =========================================================
   REVEAL ON SCROLL
   ========================================================= */

let revealObserver = null;

const vReveal = {
  mounted(el) {
    if (revealObserver) {
      revealObserver.observe(el);
    }
  },

  unmounted(el) {
    revealObserver?.unobserve(el);
  },
};

/* =========================================================
   ANIMATED STATS
   ========================================================= */

const animatedStats = ref({
  employee: 0,
  projects: 0,
  clients: 0,
});

const statsTargets = {
  employee: 16,
  projects: 50,
  clients: 19,
};

let statsAnimated = false;
let statsAnimationFrame = null;

const animateStats = () => {
  if (statsAnimated) return;

  statsAnimated = true;

  const duration = 1400;
  const start = performance.now();

  const tick = (now) => {
    const progress = Math.min((now - start) / duration, 1);
    const eased = 1 - Math.pow(1 - progress, 3);

    animatedStats.value = {
      employee: Math.round(statsTargets.employee * eased),
      projects: Math.round(statsTargets.projects * eased),
      clients: Math.round(statsTargets.clients * eased),
    };

    if (progress < 1) {
      statsAnimationFrame = requestAnimationFrame(tick);
    } else {
      animatedStats.value = {
        employee: statsTargets.employee,
        projects: statsTargets.projects,
        clients: statsTargets.clients,
      };

      statsAnimationFrame = null;
    }
  };

  statsAnimationFrame = requestAnimationFrame(tick);
};

const checkStatsVisibility = () => {
  const aboutContent = document.querySelector(".about__content");

  if (!aboutContent || statsAnimated) return;

  const rect = aboutContent.getBoundingClientRect();

  const isVisible = rect.top < window.innerHeight * 0.85 && rect.bottom > 0;

  if (isVisible) {
    animateStats();
  }
};

/* =========================================================
   SERVICES DATA — NEW
   ========================================================= */

const servicesList = [
  {
    title: "service1",
    desc: "service1_desc",
    img: "https://images.unsplash.com/photo-1586023492125-27b2c045efd7?q=80&w=600&auto=format&fit=crop",
  },
  {
    title: "service2",
    desc: "service2_desc",
    img: "https://images.unsplash.com/photo-1518005020951-eccb494ad742?q=80&w=600&auto=format&fit=crop",
  },
  {
    title: "service3",
    desc: "service3_desc",
    img: "https://images.unsplash.com/photo-1559028012-481c04fa702d?q=80&w=600&auto=format&fit=crop",
  },
  {
    title: "service4",
    desc: "service4_desc",
    img: "https://images.unsplash.com/photo-1586023492125-27b2c045efd7?q=80&w=600&auto=format&fit=crop",
  },
  {
    title: "service5",
    desc: "service5_desc",
    img: "https://images.unsplash.com/photo-1518005020951-eccb494ad742?q=80&w=600&auto=format&fit=crop",
  },
  {
    title: "service6",
    desc: "service6_desc",
    img: "https://images.unsplash.com/photo-1559028012-481c04fa702d?q=80&w=600&auto=format&fit=crop",
  },
];

/* =========================================================
   SERVICES SLIDER
   ========================================================= */

const servicesCards = ref(null);
const servicesTrack = ref(null);

let servicesAutoSlideTimer = null;
let servicesResumeTimer = null;

const servicesDrag = {
  active: false,
  startX: 0,
  startScrollLeft: 0,
};

const getServiceStep = () => {
  const slider = servicesCards.value;

  if (!slider) return 0;

  const cards = [...slider.querySelectorAll(".service-card")];

  if (!cards.length) return 0;

  const currentScroll = slider.scrollLeft;

  const nextCard = cards.find((card) => card.offsetLeft > currentScroll + 10);

  if (!nextCard) {
    return cards[0]?.offsetLeft || 0;
  }

  return nextCard.offsetLeft - currentScroll;
};

const goToNextService = () => {
  const slider = servicesCards.value;

  if (!slider) return;

  const maxScrollLeft = slider.scrollWidth - slider.clientWidth;

  if (maxScrollLeft <= 5) return;

  const step = getServiceStep();

  if (!step) return;

  if (slider.scrollLeft >= maxScrollLeft - 10) {
    slider.scrollTo({
      left: 0,
      behavior: "smooth",
    });

    return;
  }

  slider.scrollTo({
    left: slider.scrollLeft + step,
    behavior: "smooth",
  });
};

const goToPreviousService = () => {
  const slider = servicesCards.value;

  if (!slider) return;

  const maxScrollLeft = slider.scrollWidth - slider.clientWidth;

  if (maxScrollLeft <= 5) return;

  const step = getServiceStep();

  if (!step) return;

  if (slider.scrollLeft <= 10) {
    slider.scrollTo({
      left: maxScrollLeft,
      behavior: "smooth",
    });

    return;
  }

  slider.scrollBy({
    left: -step,
    behavior: "smooth",
  });
};

const startServicesAutoSlide = () => {
  stopServicesAutoSlide();

  servicesAutoSlideTimer = window.setInterval(() => {
    goToNextService();
  }, 2600);
};

const stopServicesAutoSlide = () => {
  if (servicesAutoSlideTimer) {
    window.clearInterval(servicesAutoSlideTimer);
    servicesAutoSlideTimer = null;
  }
};

const pauseServicesAutoSlide = () => {
  window.clearTimeout(servicesResumeTimer);
  stopServicesAutoSlide();
};

const resumeServicesAutoSlide = () => {
  window.clearTimeout(servicesResumeTimer);

  servicesResumeTimer = window.setTimeout(() => {
    startServicesAutoSlide();
  }, 1000);
};

/* =========================================================
   SERVICES DRAG
   ========================================================= */

const startServicesDrag = (event) => {
  if (event.pointerType === "mouse" && event.button !== 0) {
    return;
  }

  const slider = servicesCards.value;

  if (!slider) return;

  pauseServicesAutoSlide();

  servicesDrag.active = true;
  servicesDrag.startX = event.clientX;
  servicesDrag.startScrollLeft = slider.scrollLeft;

  slider.classList.add("is-dragging");

  slider.setPointerCapture?.(event.pointerId);
};

const moveServicesDrag = (event) => {
  if (!servicesDrag.active) return;

  const slider = servicesCards.value;

  if (!slider) return;

  const movedDistance = event.clientX - servicesDrag.startX;

  slider.scrollLeft = servicesDrag.startScrollLeft - movedDistance;
};

const endServicesDrag = (event) => {
  const slider = servicesCards.value;

  if (!slider || !servicesDrag.active) return;

  servicesDrag.active = false;

  slider.classList.remove("is-dragging");

  if (event?.pointerId !== undefined) {
    slider.releasePointerCapture?.(event.pointerId);
  }

  resumeServicesAutoSlide();
};

/* =========================================================
   TESTIMONIALS DATA
   ========================================================= */

const testimonials = [
  {
    text: "test1_text",
    name: "مهندس علی کیانی",
    role: "role1",
    rating: 5,
    image: "https://i.pravatar.cc/150?img=12",
  },
  {
    text: "test2_text",
    name: "مهندس رضا صادقی",
    role: "role2",
    rating: 5,
    image: "https://i.pravatar.cc/150?img=11",
  },
  {
    text: "test3_text",
    name: "دکتر آرین مطاعی",
    role: "role3",
    rating: 5,
    image: "https://i.pravatar.cc/150?img=47",
  },
  {
    text: "test4_text",
    name: "X",
    role: "role4",
    rating: 5,
    image: "https://i.pravatar.cc/150?img=32",
  },
  {
    text: "test5_text",
    name: "Y",
    role: "role5",
    rating: 5,
    image: "https://i.pravatar.cc/150?img=53",
  },
  {
    text: "test6_text",
    name: "Z",
    role: "role6",
    rating: 5,
    image: "https://i.pravatar.cc/150?img=45",
  },
];

const displayedTestimonials = computed(() => [
  ...testimonials,
  ...testimonials,
  ...testimonials,
]);

/* =========================================================
   TESTIMONIAL SLIDER
   ========================================================= */

const testimonialsCards = ref(null);

let testimonialsAutoSlideTimer = null;
let testimonialsResumeTimer = null;

const activeTestimonialDot = ref(0);

const testimonialsDrag = {
  active: false,
  startX: 0,
  startScrollLeft: 0,
};

const getTestimonialStep = () => {
  const slider = testimonialsCards.value;

  if (!slider) return 0;

  const cards = [...slider.querySelectorAll(".testimonial-card")];

  if (!cards.length) return 0;

  const currentScroll = slider.scrollLeft;

  const nextCard = cards.find((card) => card.offsetLeft > currentScroll + 10);

  if (!nextCard) {
    return cards[0]?.offsetLeft || 0;
  }

  return nextCard.offsetLeft - currentScroll;
};

const updateTestimonialActiveDot = () => {
  const slider = testimonialsCards.value;

  if (!slider) return;

  const cards = [...slider.querySelectorAll(".testimonial-card")];

  if (!cards.length || !testimonials.length) return;

  let closestIndex = 0;
  let smallestDistance = Infinity;

  cards.forEach((card, index) => {
    const distance = Math.abs(card.offsetLeft - slider.scrollLeft);

    if (distance < smallestDistance) {
      smallestDistance = distance;
      closestIndex = index;
    }
  });

  activeTestimonialDot.value = closestIndex % testimonials.length;
};

const goToNextTestimonial = () => {
  const slider = testimonialsCards.value;

  if (!slider) return;

  const maxScrollLeft = slider.scrollWidth - slider.clientWidth;

  if (maxScrollLeft <= 5) return;

  const step = getTestimonialStep();

  if (!step) return;

  if (slider.scrollLeft >= maxScrollLeft - 10) {
    slider.scrollTo({
      left: 0,
      behavior: "smooth",
    });

    activeTestimonialDot.value = 0;

    return;
  }

  slider.scrollTo({
    left: slider.scrollLeft + step,
    behavior: "smooth",
  });
};

const goToPreviousTestimonial = () => {
  const slider = testimonialsCards.value;

  if (!slider) return;

  const maxScrollLeft = slider.scrollWidth - slider.clientWidth;

  if (maxScrollLeft <= 5) return;

  const step = getTestimonialStep();

  if (!step) return;

  if (slider.scrollLeft <= 10) {
    slider.scrollTo({
      left: maxScrollLeft,
      behavior: "smooth",
    });

    return;
  }

  slider.scrollBy({
    left: -step,
    behavior: "smooth",
  });
};

const goToTestimonial = (index) => {
  const slider = testimonialsCards.value;

  if (!slider) return;

  const cards = [...slider.querySelectorAll(".testimonial-card")];

  const targetCard = cards[index];

  if (!targetCard) return;

  pauseTestimonialsAutoSlide();

  slider.scrollTo({
    left: targetCard.offsetLeft,
    behavior: "smooth",
  });

  activeTestimonialDot.value = index;

  resumeTestimonialsAutoSlide();
};

const startTestimonialsAutoSlide = () => {
  stopTestimonialsAutoSlide();

  testimonialsAutoSlideTimer = window.setInterval(() => {
    goToNextTestimonial();
  }, 3200);
};

const stopTestimonialsAutoSlide = () => {
  if (testimonialsAutoSlideTimer) {
    window.clearInterval(testimonialsAutoSlideTimer);
    testimonialsAutoSlideTimer = null;
  }
};

const pauseTestimonialsAutoSlide = () => {
  window.clearTimeout(testimonialsResumeTimer);
  stopTestimonialsAutoSlide();
};

const resumeTestimonialsAutoSlide = () => {
  window.clearTimeout(testimonialsResumeTimer);

  testimonialsResumeTimer = window.setTimeout(() => {
    startTestimonialsAutoSlide();
  }, 1200);
};

/* =========================================================
   TESTIMONIAL DRAG
   ========================================================= */

const startTestimonialsDrag = (event) => {
  if (event.pointerType === "mouse" && event.button !== 0) {
    return;
  }

  const slider = testimonialsCards.value;

  if (!slider) return;

  pauseTestimonialsAutoSlide();

  testimonialsDrag.active = true;
  testimonialsDrag.startX = event.clientX;
  testimonialsDrag.startScrollLeft = slider.scrollLeft;

  slider.classList.add("is-dragging");

  slider.setPointerCapture?.(event.pointerId);
};

const moveTestimonialsDrag = (event) => {
  if (!testimonialsDrag.active) return;

  const slider = testimonialsCards.value;

  if (!slider) return;

  const movedDistance = event.clientX - testimonialsDrag.startX;

  slider.scrollLeft = testimonialsDrag.startScrollLeft - movedDistance;
};

const endTestimonialsDrag = (event) => {
  const slider = testimonialsCards.value;

  if (!slider || !testimonialsDrag.active) return;

  testimonialsDrag.active = false;

  slider.classList.remove("is-dragging");

  if (event?.pointerId !== undefined) {
    slider.releasePointerCapture?.(event.pointerId);
  }

  updateTestimonialActiveDot();

  resumeTestimonialsAutoSlide();
};

/* =========================================================
   TRANSLATIONS
   ========================================================= */

const translations = {
  fa: {
    About: "درباره",
    Services: "خدمات",
    "Our Expertise": "تخصص ما",
    "Case Studies": "پروژه‌ها",
    "Contact Us": "تماس با ما",
    Expertise: "تخصص",

    Create: "راهکارهای هوشمند",
    "Business Solution": "برای سازمان شما",

    "Lorem ipsum dolor sit amet consectetur adipisicing":
      "با بهره‌گیری از فناوری‌های پیشرفته و تیم متخصص",

    "WRITE TO US": "تماس با ما",

    Us: "ما",

    "Nulla lobortis nunc vitae nisi semper semper velit":
      "رایبد پویه شرکتی پیشرو در زمینه مشاوره و تولید نرم‌افزارهای هوشمند است، که به توسعه و ارائه راهکارهای نوآورانه در زمینه انواع ارتباطات شبکه‌ای می‌پردازد.",

    Employee: "سال تجربه",
    Projects: "پروژه موفق",
    Clients: "سازمان بزرگ",

    "Aliquam lobortis magna neque, gravida consequat velit venenatis at.":
      "با بهره‌گیری از تکنولوژی‌های پیشرفته و تیم متخصص، ما خدمات و محصولات هوشمندی ارائه می‌کنیم که به سازمان‌ها کمک می‌کند تا عملکردهای خود را بهبود ببخشند.",

    Our: "ما",

    service1: "نرم‌افزار مدیریت هوشمند شبکه",
    service1_desc:
      "نرم‌افزاری جامع برای مدیریت و پایش هوشمند شبکه‌های سازمانی با قابلیت عیب‌یابی خودکار.",
    service2: "موتور جستجوی هوشمند و دستیار GPT",
    service2_desc:
      "موتور جستجوی هوشمند همراه با دستیار GPT که امکان دستیابی سریع به اطلاعات را فراهم می‌کند.",
    service3: "شبکه دانش هوشمند رایا",
    service3_desc:
      "شبکه دانش هوشمند رایا، بستری برای مدیریت و تبادل دانش در سازمان.",
    service4: "سامانه تحلیل داده",
    service4_desc:
      "سامانه پیشرفته تحلیل داده‌های سازمانی با گزارش‌های تعاملی و دقیق.",
    service5: "پلتفرم یکپارچه سازمانی",
    service5_desc:
      "پلتفرم یکپارچه جهت اتصال سامانه‌های مختلف سازمان و تسهیل جریان داده.",
    service6: "راهکار امنیت شبکه",
    service6_desc:
      "راهکار جامع امنیت شبکه با قابلیت شناسایی تهدیدات و پاسخ سریع به حملات.",

    "SEE DETAIL": "مشاهده جزئیات",

    Marketing: "مدیریت دانش",
    SEO: "هوش مصنوعی",
    "Social Media": "شبکه‌های عصبی",
    "Web Development": "توسعه نرم‌افزار",
    "UI Design": "طراحی سیستم",
    "Mobile Apps": "اپلیکیشن موبایل",
    Photography: "پشتیبانی فنی",
    "Company Profile": "امنیت شبکه",
    "Visual Editing": "پایگاه داده",

    "Curabitur egestas consequat lorem, vel fermentum augue porta id. Aliquam lobortis magna neque, gravida consequat velit venenatis at. Duis sed augue.":
      "ما با تکیه بر فناوری‌های روز دنیا و هوش مصنوعی، راهکارهایی هوشمند، امن و متناسب با زیرساخت سازمان ارائه می‌دهیم.",

    Client: "مشتریان",
    Testimonials: "نظرات",

    test1_text:
      "تفاوت اصلی این پروژه برای ما، شروع مدیریت دانش از مسئله‌های واقعی سازمان بود، نه از انتخاب ابزار و نرم‌افزار. تیم مشاور با شناخت دقیق فرایندها و چالش‌های سازمان، راهکارهایی متناسب با نیازهای ما طراحی و اجرا کرد.",

    test2_text:
      "یکی از نقاط قوت همکاری، رویکرد کاملاً اجرایی تیم مشاور بود. خروجی پروژه فقط مجموعه‌ای از مستندات نبود؛ بلکه فرایندها، نقش‌ها و سازوکارهایی ایجاد شد که امکان ادامه و توسعه مدیریت دانش را در سازمان فراهم می‌کند.",

    test3_text:
      "پیش از استفاده از سامانه، شناسایی منشأ اختلالات شبکه زمان زیادی از کارشناسان می‌گرفت. داشبوردها و اطلاعات متمرکز نرم‌افزار، فرآیند تشخیص و رفع مشکل را برای تیم ما بسیار سریع‌تر کرده است.",

    test4_text:
      "برای ما امنیت و کنترل داده‌ها اهمیت بالایی داشت. استفاده از یک راهکار بومی که امکان استقرار در زیرساخت سازمان را فراهم می‌کند، باعث شد بتوانیم مدیریت و پایش شبکه را بدون وابستگی به سرویس‌های خارجی انجام دهیم.",

    test5_text:
      "حجم بالای اسناد و اطلاعات سازمان باعث شده بود پیدا کردن اطلاعات موردنیاز زمان زیادی از کارشناسان بگیرد. موتور جستجو و دستیار هوشمند رایا این امکان را فراهم کرد که اطلاعات موردنیاز را سریع‌تر و دقیق‌تر، از میان منابع مختلف سازمان پیدا کنیم.",

    test6_text:
      "مزیت اصلی موتور جستجوی رایا برای ما این است که جستجو فقط بر اساس تطابق کلمات انجام نمی‌شود؛ سیستم می‌تواند مفهوم و ارتباط محتوای موردنظر را نیز درک کند و نتایج مرتبط‌تری در اختیار کاربر قرار دهد.",

    role1: "رئیس سیستم‌های کیفیت و سرآمدی، شرکت فولاد مبارکه اصفهان",

    role2: "مدیر مهندسی صنایع، شرکت فولاد سنگان",

    role3: "رئیس فناوری اطلاعات، اداره کل کتابخانه‌های عمومی استان کرمانشاه",

    role4: "مدیر فناوری اطلاعات",
    role5: "مدیر دانش",
    role6: "کارشناس ارشد فناوری",

    Case: "پروژه",
    Studies: "ها",
    Corporate: "سازمانی",
    Advertising: "دولتی",
    Government: "صنعتی",
    Creative: "خدماتی",

    "Vestibulum consequat hendrerit.": "مشاهده جزئیات پروژه",

    proj1: "پروژه مدیریت دانش فولاد مبارکه",
    proj2: "پروژه شبکه هوشمند فولاد سنگان",
    proj3: "سامانه مدیریت کتابخانه‌های عمومی",
    proj4: "موتور جستجوی سازمانی",

    "Ready to get started ?": "آماده همکاری هستید؟",

    "CONTACT US": "تماس با ما",

    "Pellentesque ac bibendum tortor. Nulla eget lobortis lacus.":
      "برای شروع همکاری و دریافت مشاوره با ما تماس بگیرید.",

    Office: "دفتر",
    "Head Quarter": "دفتر مرکزی",
    "Branch Office": "شعبه",

    "+123 456 789 01": "+۹۸-۲۱-۱۲۳۴۵۶۷۸",

    "+98 765 432 10": "+۹۸-۲۱-۷۶۵۴۳۲۱۰",

    "Lorem ipsum street no 14 Block A":
      "تهران، دانشگاه تهران، خیابان قدس، کوچه آذین، پلاک ۴",

    "Vivamus street Block C - Vestibulum Building - 3rd Floor":
      "تهران، خیابان ولیعصر، پلاک ۱۲۳، طبقه ۳",

    COMPANY: "شرکت",
    SERVICES: "خدمات",
    RESOURCES: "منابع",

    "English - En": "English - En",
    "Persian - Fa": "فارسی - Fa",

    "Nam posuere accumsan porta. Integer id orci sed ante tincidunt tincidunt at sed libero.":
      "رایبد پویه با ۱۶ سال تجربه و بیش از ۵۰ پروژه موفق، همراه مطمئن سازمان‌ها در مسیر تحول دیجیتال.",

    "© Lu Theme 2019": "© رایبد پویه ۱۴۰۵",

    "Donec dignissim": "درباره ما",
    "Curabitur egestas": "خدمات",
    "Nam posuere": "تخصص ما",
    "Aenean facilisis": "نمونه کارها",

    "Cras convallis": "مشاوره مدیریت دانش",

    "Vestibulum faucibus": "توسعه نرم‌افزارهای هوشمند",

    "Quisque lacinia purus": "پیاده‌سازی سیستم‌های دانش",

    "Aliquam nec ex": "پشتیبانی و آموزش",

    "Suspendisse porttitor": "مستندات",

    "Services ◆": "خدمات ◆",

    "hello@lulu.com": "info@raybidpouye.com",

    BrandName: "رایبد پویه",
  },

  en: {
    About: "About",
    Services: "Services",
    "Our Expertise": "Our Expertise",
    "Case Studies": "Projects",
    "Contact Us": "Contact Us",
    Expertise: "Expertise",

    Create: "Smart Solutions",
    "Business Solution": "for Your Organization",

    "Lorem ipsum dolor sit amet consectetur adipisicing":
      "Leveraging advanced technologies and expert team",

    "WRITE TO US": "Contact Us",

    Us: "Us",

    "Nulla lobortis nunc vitae nisi semper semper velit":
      "Raybod Pouye is a leading company in consulting and development of intelligent software, providing innovative solutions in network communications.",

    Employee: "Years of Experience",
    Projects: "Successful Projects",
    Clients: "Major Organizations",

    "Aliquam lobortis magna neque, gravida consequat velit venenatis at.":
      "Leveraging advanced technologies and a dedicated expert team, we provide intelligent services and products that help organizations improve their performance.",

    Our: "Our",

    service1: "Intelligent Network Management Software",
    service1_desc:
      "Proin ac arcu nisl. Duis eu molestie lectus. Nam quis mauris faucibus, aliquet elit eu, rhoncus ipsum.",
    service2: "Intelligent Search Engine & GPT Assistant",
    service2_desc:
      "Proin quis pellentesque dui. Ut sed leo neque. Nullam aliquet iaculis neque a commodo.",
    service3: "Raya Intelligent Knowledge Network",
    service3_desc:
      "Quisque consectetur lectus vel orci porttitor gravida ac eu erat. Nullam accumsan nibh tortor.",
    service4: "Data Analytics Platform",
    service4_desc:
      "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor.",
    service5: "Enterprise Integration Platform",
    service5_desc:
      "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia.",
    service6: "Network Security Solution",
    service6_desc:
      "Nullam quis mauris faucibus. Aliquam elit eu rhoncus. Proin ac arcu nisl.",

    "SEE DETAIL": "See Details",

    Marketing: "Knowledge Management",
    SEO: "Artificial Intelligence",
    "Social Media": "Neural Networks",
    "Web Development": "Software Development",
    "UI Design": "System Design",
    "Mobile Apps": "Mobile Apps",
    Photography: "Technical Support",
    "Company Profile": "Network Security",
    "Visual Editing": "Databases",

    "Curabitur egestas consequat lorem, vel fermentum augue porta id. Aliquam lobortis magna neque, gravida consequat velit venenatis at. Duis sed augue.":
      "We provide intelligent, secure solutions tailored to organizational infrastructure using cutting-edge technologies and AI.",

    Client: "Clients",
    Testimonials: "Testimonials",

    test1_text:
      "The main difference of this project for us was starting knowledge management from real organizational problems, not from choosing tools. The consulting team designed and implemented solutions tailored to our needs with precise understanding of our processes and challenges.",

    test2_text:
      "One of the strengths of this collaboration was the fully practical approach of the consulting team. The project output was not just a set of documents; processes, roles, and mechanisms were established to enable the continuation and development of knowledge management in the organization.",

    test3_text:
      "Before using the system, identifying the source of network disruptions took a lot of time from our experts. The dashboards and centralized information of the software have made the diagnosis and resolution process much faster for our team.",

    test4_text:
      "For us, data security and control were very important. Using a native solution that can be deployed on organizational infrastructure allowed us to manage and monitor the network without relying on external services.",

    test5_text:
      "The high volume of organizational documents and information made it time-consuming for experts to find needed information. Raya's search engine and intelligent assistant enabled us to find information faster and more accurately from various organizational sources.",

    test6_text:
      "The main advantage of Raya's search engine for us is that search is not just based on word matching; the system can understand the meaning and relationship of the content and provide more relevant results to the user.",

    role1: "Head of Quality and Excellence Systems, Mobarakeh Steel Company",

    role2: "Industrial Engineering Manager, Sangan Steel Company",

    role3:
      "IT Director, General Directorate of Public Libraries of Kermanshah Province",

    role4: "IT Manager",
    role5: "Knowledge Manager",
    role6: "Senior Technology Expert",

    Case: "Projects",
    Studies: "",
    Corporate: "Corporate",
    Advertising: "Government",
    Government: "Industrial",
    Creative: "Service",

    "Vestibulum consequat hendrerit.": "View Project Details",

    proj1: "Mobarakeh Steel Knowledge Management Project",

    proj2: "Sangan Steel Intelligent Network Project",

    proj3: "Public Libraries Management System",

    proj4: "Enterprise Search Engine",

    "Ready to get started ?": "Ready to collaborate?",

    "CONTACT US": "Contact Us",

    "Pellentesque ac bibendum tortor. Nulla eget lobortis lacus.":
      "Contact us to start collaboration and get consultation.",

    Office: "Office",
    "Head Quarter": "Headquarters",
    "Branch Office": "Branch",

    "+123 456 789 01": "+98-21-12345678",

    "+98 765 432 10": "+98-21-76543210",

    "Lorem ipsum street no 14 Block A":
      "Tehran, University of Tehran, Qods St., Azin Alley, No. 4",

    "Vivamus street Block C - Vestibulum Building - 3rd Floor":
      "Tehran, Valiasr St., No. 123, 3rd Floor",

    COMPANY: "Company",
    SERVICES: "Services",
    RESOURCES: "Resources",

    "English - En": "English - En",
    "Persian - Fa": "Persian - Fa",

    "Nam posuere accumsan porta. Integer id orci sed ante tincidunt tincidunt at sed libero.":
      "Raybod Pouye with 16 years of experience and over 50 successful projects, a trusted partner for organizations on their digital transformation journey.",

    "© Lu Theme 2019": "© Raybod Pouye 2026",

    "Donec dignissim": "About Us",
    "Curabitur egestas": "Services",
    "Nam posuere": "Our Expertise",
    "Aenean facilisis": "Projects",

    "Cras convallis": "Knowledge Management Consulting",

    "Vestibulum faucibus": "Intelligent Software Development",

    "Quisque lacinia purus": "Knowledge Systems Implementation",

    "Aliquam nec ex": "Support and Training",

    "Suspendisse porttitor": "Documentation",

    "Services ◆": "Services ◆",

    "hello@lulu.com": "info@raybidpouye.com",

    BrandName: "Raybod Pouye",
  },
};

/* =========================================================
   TRANSLATION FUNCTION
   ========================================================= */

const t = (key) => {
  return translations[currentLanguage.value]?.[key] ?? key;
};

/* =========================================================
   LANGUAGE SWITCH
   ========================================================= */

const toggleLanguage = () => {
  if (isSwitchingLanguage.value) return;

  isSwitchingLanguage.value = true;

  window.setTimeout(() => {
    currentLanguage.value = currentLanguage.value === "en" ? "fa" : "en";

    nextTick(() => {
      checkStatsVisibility();
      alignTestimonialSlider();
    });

    window.setTimeout(() => {
      isSwitchingLanguage.value = false;
    }, 30);
  }, 180);
};

/* =========================================================
   NAVIGATION
   ========================================================= */

const navItems = [
  {
    label: "About",
    href: "#about",
  },
  {
    label: "Services",
    href: "#services",
  },
  {
    label: "Our Expertise",
    href: "#expertise",
  },
  {
    label: "Case Studies",
    href: "#case-studies",
  },
  {
    label: "Contact Us",
    href: "#contact",
  },
];

/* =========================================================
   CASE STUDIES
   ========================================================= */

const projects = [
  {
    title: "proj1",
    class: "case-card--large",
    image:
      "https://images.unsplash.com/photo-1497366754035-f200968a6e72?q=80&w=1000&auto=format&fit=crop",
  },
  {
    title: "proj2",
    class: "case-card--logo",
    logo: true,
  },
  {
    title: "proj3",
    class: "case-card--small",
    image:
      "https://images.unsplash.com/photo-1497366811353-6870744d04b2?q=80&w=800&auto=format&fit=crop",
  },
  {
    title: "proj4",
    class: "case-card--wide",
    image:
      "https://images.unsplash.com/photo-1497366216548-37526070297c?q=80&w=1000&auto=format&fit=crop",
  },
];

/* =========================================================
   SCROLLSPY
   ========================================================= */

const sectionIds = [
  "home",
  "about",
  "services",
  "expertise",
  "testimonials",
  "case-studies",
  "contact",
];

const handleScroll = () => {
  isScrolled.value = window.scrollY > 40;

  checkStatsVisibility();

  const scrollPos = window.scrollY + 140;

  let current = sectionIds[0];

  for (const id of sectionIds) {
    const el = document.getElementById(id);

    if (el && el.offsetTop <= scrollPos) {
      current = id;
    }
  }

  activeSection.value = current;
};

/* =========================================================
   SCROLL TO TOP
   ========================================================= */

const scrollTop = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};

/* =========================================================
   ALIGN TESTIMONIAL SLIDER ON LOAD
   ========================================================= */

const alignTestimonialSlider = () => {
  const slider = testimonialsCards.value;

  const track = slider?.querySelector(".testimonials__track");

  if (!slider || !track) return;

  const style = getComputedStyle(track);

  const paddingProp =
    currentLanguage.value === "fa" ? "paddingRight" : "paddingLeft";

  const paddingValue = parseFloat(style[paddingProp]);

  if (!isNaN(paddingValue)) {
    slider.scrollLeft = paddingValue;
  }
};

/* =========================================================
   RESPONSIVE MENU
   ========================================================= */

let resizeMenuLockTimer = null;

const handleResize = () => {
  isMenuOpen.value = false;

  document.documentElement.classList.add("is-resizing");

  if (resizeMenuLockTimer) {
    window.clearTimeout(resizeMenuLockTimer);
  }

  resizeMenuLockTimer = window.setTimeout(() => {
    document.documentElement.classList.remove("is-resizing");

    resizeMenuLockTimer = null;
  }, 120);
};

/* =========================================================
   DOCUMENT CLICK
   ========================================================= */

const handleDocumentClick = (event) => {
  if (!isMenuOpen.value) return;

  const target = event.target;

  const nav = document.querySelector(".nav");

  const menuButton = document.querySelector(".menu-btn");

  if (nav?.contains(target) || menuButton?.contains(target)) {
    return;
  }

  isMenuOpen.value = false;
};

/* =========================================================
   LIFECYCLE
   ========================================================= */

onMounted(async () => {
  document.documentElement.classList.add("ray-loader-active");

  const minimumIntro = new Promise((resolve) =>
    window.setTimeout(resolve, 2800),
  );

  const fontsReady = document.fonts?.ready ?? Promise.resolve();

  await Promise.all([minimumIntro, fontsReady]);

  await new Promise((resolve) => requestAnimationFrame(resolve));

  isInitialLoading.value = false;

  document.documentElement.classList.remove("ray-loader-active");

  if ("IntersectionObserver" in window) {
    revealObserver = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (!entry.isIntersecting) return;

          entry.target.classList.add("reveal--visible");

          if (entry.target.classList.contains("about__content")) {
            animateStats();
          }

          revealObserver?.unobserve(entry.target);
        });
      },
      {
        threshold: 0.18,
        rootMargin: "0px 0px -60px 0px",
      },
    );
  }

  await nextTick();

  alignTestimonialSlider();

  handleScroll();

  checkStatsVisibility();

  window.addEventListener("scroll", handleScroll, { passive: true });

  window.addEventListener("resize", handleResize, { passive: true });

  document.addEventListener("click", handleDocumentClick);

  if (revealObserver) {
    document.querySelectorAll("[data-v-reveal], .reveal").forEach((el) => {
      revealObserver.observe(el);
    });
  }

  window.setTimeout(() => {
    startServicesAutoSlide();

    startTestimonialsAutoSlide();

    updateTestimonialActiveDot();

    checkStatsVisibility();
  }, 500);

  testimonialsCards.value?.addEventListener(
    "scroll",
    updateTestimonialActiveDot,
    { passive: true },
  );
});

/* =========================================================
   CLEANUP
   ========================================================= */

onUnmounted(() => {
  document.documentElement.classList.remove("ray-loader-active");

  window.removeEventListener("scroll", handleScroll);

  window.removeEventListener("resize", handleResize);

  document.removeEventListener("click", handleDocumentClick);

  if (resizeMenuLockTimer) {
    window.clearTimeout(resizeMenuLockTimer);

    resizeMenuLockTimer = null;
  }

  document.documentElement.classList.remove("is-resizing");

  testimonialsCards.value?.removeEventListener(
    "scroll",
    updateTestimonialActiveDot,
  );

  stopServicesAutoSlide();

  stopTestimonialsAutoSlide();

  window.clearTimeout(servicesResumeTimer);

  window.clearTimeout(testimonialsResumeTimer);

  if (statsAnimationFrame) {
    cancelAnimationFrame(statsAnimationFrame);

    statsAnimationFrame = null;
  }

  revealObserver?.disconnect();

  revealObserver = null;
});
</script>

<style>
/* ==============================
   1. FONTS & CSS VARIABLES
   ============================== */
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=Vazirmatn:wght@400;500;600;700;800;900&display=swap");

:root {
  --blue: #269ff3;
  --blue-dark: #0751af;
  --blue-soft: #eaf7ff;
  --cyan: #9eeff3;
  --text: #242833;
  --muted: #8b96a6;
  --light: #f1f9ff;
  --white: #ffffff;
  --shadow: 0 18px 45px rgba(18, 77, 126, 0.13);
  --shadow-soft: 0 10px 30px rgba(17, 80, 140, 0.12);
  --radius: 18px;
  --container: 1180px;
  --ease: cubic-bezier(0.16, 1, 0.3, 1);
}

/* ==============================
   2. GLOBAL RESET & BASE
   ============================== */
* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  font-family: "Inter", sans-serif;
  color: var(--text);
  background: #ffffff;
  overflow-x: hidden;
}

.app.is-rtl,
.app.is-rtl body {
  font-family: "Vazirmatn", "Inter", sans-serif;
}

.app.is-rtl {
  font-family: "Vazirmatn", "Inter", sans-serif;
}

a {
  color: inherit;
  text-decoration: none;
}

button,
select {
  font-family: inherit;
}

img {
  max-width: 100%;
  display: block;
}

::selection {
  background: var(--blue);
  color: #fff;
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.001ms !important;
    scroll-behavior: auto !important;
  }
}

/* ==============================
   3. UTILITY / LAYOUT CLASSES
   ============================== */
.app {
  width: 100%;
  overflow: hidden;
}

.container {
  width: min(var(--container), calc(100% - 52px));
  margin-inline: auto;
}

.section {
  position: relative;
}

/* Scroll-reveal system */
.reveal {
  opacity: 0;
  transform: translateY(36px);
  transition:
    opacity 0.8s var(--ease),
    transform 0.8s var(--ease);
  will-change: opacity, transform;
}

.reveal--left {
  transform: translateX(-46px);
}

.reveal--right {
  transform: translateX(46px);
}

.reveal--up {
  transform: translateY(28px);
}

.reveal--visible {
  opacity: 1;
  transform: translate(0, 0);
}

/* ==============================
   4. HEADER
   ============================== */
.header {
  position: fixed;
  inset: 0 0 auto 0;
  z-index: 100;
  height: 76px;
  transition: 0.3s ease;
}

.header.scrolled {
  background: rgba(255, 255, 255, 0.92);
  box-shadow: 0 10px 30px rgba(20, 95, 160, 0.08);
  backdrop-filter: blur(16px);
}

.header__inner {
  height: 100%;
  display: flex;
  align-items: center;
  gap: 38px;
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
  font-weight: 800;
  color: #2a2c32;
  flex-shrink: 0;
  transition: transform 0.25s var(--ease);
}

.brand:hover {
  transform: translateY(-1px);
}

.brand__mark {
  width: 48px;
  height: 48px;
  color: white;
  display: grid;
  place-items: center;
  background: linear-gradient(135deg, #14b3ff, #1679e9);
  border-radius: 14px;
  transform: rotate(45deg);
  font-weight: 900;
  letter-spacing: -0.5px;
  line-height: 1;
  position: relative;
  box-shadow: 0 14px 28px rgba(38, 159, 243, 0.25);
  isolation: isolate;
  overflow: hidden;
  text-align: center;
  user-select: none;
  font-size: 0;
  transition:
    transform 0.35s var(--ease),
    box-shadow 0.35s var(--ease);
}

.brand:hover .brand__mark {
  transform: rotate(45deg) scale(1.08);
  box-shadow: 0 16px 34px rgba(38, 159, 243, 0.35);
}

.brand__mark::after {
  content: "RP";
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: 900;
  color: white;
  transform: rotate(-45deg);
}

.brand__text {
  font-size: 22px;
}

.nav {
  display: flex;
  align-items: center;
  gap: 38px;
  margin-left: auto;
}

.app.is-rtl .nav {
  margin-left: 0;
  margin-right: auto;
}

.nav a {
  position: relative;
  font-size: 13px;
  font-weight: 700;
  color: white;
  transition: 0.25s;
  padding-bottom: 4px;
}

.nav a::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: -4px;
  height: 2px;
  border-radius: 4px;
  background: var(--cyan);
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.3s var(--ease);
}

.nav a:hover::after,
.nav a.is-active::after {
  transform: scaleX(1);
}

.header.scrolled .nav a {
  color: #345;
}

.header.scrolled .nav a::after {
  background: var(--blue);
}

.nav a:hover,
.nav a.is-active {
  color: var(--cyan);
}

/* Language switch used inside the responsive menu */
.nav-language {
  display: none;
}

.header.scrolled .nav a:hover,
.header.scrolled .nav a.is-active {
  color: var(--blue-dark);
}

.setting-btn {
  width: 42px;
  height: 42px;
  border: 0;
  background: white;
  border-radius: 50%;
  color: var(--blue-dark);
  display: grid;
  place-items: center;
  font-size: 15px;
  font-weight: 800;
  box-shadow: 0 8px 22px rgba(23, 81, 138, 0.13);
  cursor: pointer;
  transition:
    transform 0.3s var(--ease),
    box-shadow 0.3s var(--ease);
  overflow: hidden;
}

.setting-btn:hover {
  transform: translateY(-2px) rotate(8deg);
  box-shadow: 0 12px 26px rgba(23, 81, 138, 0.22);
}

.setting-btn__label {
  display: inline-block;
  animation: settingPop 0.35s var(--ease);
}

@keyframes settingPop {
  from {
    opacity: 0;
    transform: scale(0.6) rotate(-10deg);
  }
  to {
    opacity: 1;
    transform: scale(1) rotate(0);
  }
}

/* ===== NEW: Language toggle button for desktop ===== */
.lang-btn {
  width: 42px;
  height: 42px;
  border: 0;
  background: white;
  border-radius: 50%;
  color: var(--blue-dark);
  display: grid;
  place-items: center;
  font-size: 15px;
  font-weight: 800;
  box-shadow: 0 8px 22px rgba(23, 81, 138, 0.13);
  cursor: pointer;
  transition:
    transform 0.3s var(--ease),
    box-shadow 0.3s var(--ease);
  overflow: hidden;
  flex-shrink: 0;
}

.lang-btn:hover {
  transform: translateY(-2px) rotate(8deg);
  box-shadow: 0 12px 26px rgba(23, 81, 138, 0.22);
}

/* Hide language button on mobile/tablet */
@media (max-width: 992px) {
  .lang-btn {
    display: none !important;
  }
}

@media (min-width: 769px) and (max-width: 1169px) {
  .lang-btn {
    display: none !important;
  }
}

@media (max-width: 768px) {
  .lang-btn {
    display: none !important;
  }
}

.menu-btn {
  width: 44px;
  height: 38px;
  border: 0;
  background: transparent;
  display: none;
  flex-direction: column;
  justify-content: center;
  gap: 6px;
  cursor: pointer;
  margin-left: auto;
}

.menu-btn span {
  width: 28px;
  height: 3px;
  border-radius: 20px;
  background: var(--blue-dark);
  transition:
    transform 0.3s var(--ease),
    opacity 0.2s ease;
}

/* ==============================
   5. HERO
   ============================== */
.hero {
  min-height: 760px;
  padding-top: 76px;
}

.hero::before {
  content: "";
  position: absolute;
  right: -110px;
  top: 420px;
  width: 360px;
  height: 360px;
  background: rgba(124, 224, 238, 0.21);
  border-radius: 55px;
  transform: rotate(45deg);
}

.hero::after {
  content: "";
  position: absolute;
  right: 44px;
  top: 525px;
  width: 185px;
  height: 185px;
  background: rgba(225, 243, 255, 0.9);
  border-radius: 52px;
  transform: rotate(45deg);
}

.hero__image {
  position: absolute;
  top: -130px;
  right: -60px;
  width: 74vw;
  max-width: 1160px;
  height: 720px;
  overflow: hidden;
  border-radius: 0 0 130px 130px;
  border-bottom-left-radius: 130px;
  border-bottom-right-radius: 130px;
  transform: rotate(42deg);
  transform-origin: center;
  border: 42px solid var(--blue);
  border-top: 0;
  border-right: 0;
  box-shadow: 0 28px 60px rgba(24, 136, 222, 0.22);
  z-index: 0;
  animation: heroImageIn 1.1s var(--ease) both;
}

@keyframes heroImageIn {
  from {
    opacity: 0;
    transform: rotate(42deg) scale(0.94);
  }
  to {
    opacity: 1;
    transform: rotate(42deg) scale(1);
  }
}

.hero__image img {
  width: 132%;
  height: 132%;
  object-fit: cover;
  transform: rotate(-42deg) translate(-130px, 30px);
  filter: none;
}

.hero__image::after {
  display: none;
  content: none;
}

.hero__content {
  position: relative;
  z-index: 2;
  min-height: 660px;
  display: flex;
  align-items: center;
}

.hero__text {
  margin-top: 80px;
  width: 380px;
}

.hero__text.reveal {
  transform: translateY(24px);
  transition-delay: 0.15s;
}

.hero h1 {
  font-size: clamp(42px, 4.4vw, 62px);
  line-height: 1.19;
  letter-spacing: -2px;
  margin: 0 0 24px;
  font-weight: 900;
}

.hero p {
  font-size: 22px;
  color: #8c919a;
  line-height: 1.55;
  margin: 0 0 34px;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 18px;
  min-height: 54px;
  padding: 0 28px;
  border-radius: 5px;
  font-size: 13px;
  font-weight: 900;
  letter-spacing: 0.3px;
  transition: 0.3s var(--ease);
  position: relative;
  overflow: hidden;
}

.btn--primary {
  color: #03a3c4;
  background: #ffffff;
  border: 1px solid #d2edf6;
  box-shadow: 0 14px 28px rgba(28, 137, 199, 0.13);
}

.btn--primary span {
  transition: transform 0.3s var(--ease);
}

.app.is-rtl .btn--primary span {
  display: inline-block;
  transform: scaleX(-1);
}

.btn--primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 18px 34px rgba(28, 137, 199, 0.18);
  border-color: var(--blue);
}

.btn--primary:hover span {
  transform: translateX(4px);
}

.app.is-rtl .btn--primary:hover span {
  transform: scaleX(-1) translateX(4px);
}

.btn--primary:active {
  transform: translateY(-1px) scale(0.98);
}

/* Hero scroll cue */
.hero__scroll-cue {
  position: absolute;
  left: 50%;
  bottom: 28px;
  z-index: 3;
  width: 26px;
  height: 42px;
  border: 2px solid rgba(38, 159, 243, 0.35);
  border-radius: 20px;
  transform: translateX(-50%);
}

.hero__scroll-cue span {
  position: absolute;
  top: 7px;
  left: 50%;
  width: 4px;
  height: 8px;
  border-radius: 4px;
  background: var(--blue);
  transform: translateX(-50%);
  animation: scrollCue 1.8s ease-in-out infinite;
}

@keyframes scrollCue {
  0% {
    transform: translate(-50%, 0);
    opacity: 1;
  }
  70% {
    transform: translate(-50%, 14px);
    opacity: 0;
  }
  100% {
    transform: translate(-50%, 0);
    opacity: 0;
  }
}

@media (max-width: 992px) {
  .hero__scroll-cue {
    display: none;
  }
}

/* ==============================
   6. ABOUT
   ============================== */
.about {
  padding: 30px 0 120px;
}

.about::before {
  content: "";
  position: absolute;
  left: -110px;
  top: 115px;
  width: 310px;
  height: 310px;
  border-radius: 48px;
  background: rgba(228, 246, 255, 0.95);
  transform: rotate(45deg);
}

.about__inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  min-height: 420px;
  gap: 40px;
}

.about__visual {
  position: relative;
  height: 420px;
}

.about-card {
  position: absolute;
  left: 75px;
  top: 65px;
  width: 380px;
  height: 280px;
}

.diamond-title {
  width: 196px;
  height: 196px;
  display: grid;
  place-items: center;
  background: linear-gradient(135deg, #17b3fb, #2389ef);
  border-radius: 28px;
  transform: rotate(45deg);
  color: white;
  position: relative;
  z-index: 2;
  box-shadow: 0 16px 32px rgba(28, 144, 232, 0.23);
  transition:
    transform 0.4s var(--ease),
    box-shadow 0.4s var(--ease);
}

.about-card:hover .diamond-title {
  transform: rotate(45deg) scale(1.04);
  box-shadow: 0 20px 40px rgba(28, 144, 232, 0.32);
}

.diamond-title span {
  transform: rotate(-45deg);
  font-size: 36px;
  line-height: 1.15;
  font-weight: 900;
  letter-spacing: -1px;
}

.diamond-image {
  position: absolute;
  top: -28px;
  left: 140px;
  width: 205px;
  height: 205px;
  border-radius: 32px;
  overflow: hidden;
  transform: rotate(45deg);
  z-index: 1;
  box-shadow: var(--shadow);
  transition: transform 0.4s var(--ease);
}

.about-card:hover .diamond-image {
  transform: rotate(45deg) scale(1.04);
}

.diamond-image img {
  width: 160%;
  height: 195%;
  max-width: none;
  object-fit: cover;
  transform: rotate(-45deg) translate(30px, -48px);
  transition: transform 0.6s var(--ease);
}

.soft-diamond {
  position: absolute;
  border-radius: 42px;
  transform: rotate(45deg);
}

.soft-diamond--one {
  width: 250px;
  height: 250px;
  background: rgba(224, 246, 255, 0.86);
  left: -45px;
  top: 105px;
}

.about__content {
  max-width: 530px;
  padding-top: 30px;
}

.about__content h2,
.expertise__content h2 {
  font-size: 25px;
  line-height: 1.45;
  letter-spacing: -0.7px;
  margin: 0 0 35px;
  font-weight: 900;
  max-width: 450px;
}

.stats {
  display: flex;
  gap: 62px;
  margin-bottom: 44px;
}

.stat strong {
  display: flex;
  align-items: baseline;
  font-size: 32px;
  color: #111b2c;
  font-weight: 900;
  font-variant-numeric: tabular-nums;
}

.stat__plus {
  font-size: 20px;
  color: var(--blue);
  margin-left: 2px;
}

.app.is-rtl .stat__plus {
  margin-left: 0;
  margin-right: 2px;
}

.stat strong::after {
  content: "";
  display: block;
  width: 42px;
  height: 5px;
  border-radius: 20px;
  background: var(--blue);
  margin: 5px 0 8px;
}

.stat span {
  color: #9aa3af;
  font-size: 16px;
}

blockquote {
  margin: 0;
  padding-left: 28px;
  border-left: 4px solid #d9d9d9;
  color: #b6bbc3;
  font-size: 22px;
  line-height: 1.45;
  font-weight: 700;
  font-style: italic;
}

.app.is-rtl blockquote {
  padding-left: 0;
  padding-right: 28px;
  border-left: 0;
  border-right: 4px solid #d9d9d9;
  font-style: normal;
}

/* ==============================
   7. SERVICES – NEW DESIGN
   ============================== */
.services {
  background: #eef9ff;
  padding: 80px 0 90px;
  overflow: hidden;
}

.services__floating-title {
  position: relative;
  z-index: 5;
  margin-bottom: 20px;
}

.services__title-wrapper {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 20px 40px;
}

.services__title-icon {
  display: flex;
  align-items: center;
  gap: 16px;
}

.services__icon {
  display: inline-block;
  font-size: 32px;
  color: var(--blue);
  line-height: 1;
}

.services__title-icon h2 {
  margin: 0;
  font-size: clamp(32px, 4vw, 48px);
  line-height: 1.15;
  font-weight: 900;
  color: var(--blue-dark);
}

.services__arrows {
  display: flex;
  gap: 12px;
  margin-top: 48px;
}

.services__arrow-btn {
  width: 48px;
  height: 48px;
  border: 0;
  border-radius: 50%;
  background: white;
  color: var(--blue-dark);
  font-size: 28px;
  line-height: 1;
  box-shadow: var(--shadow-soft);
  cursor: pointer;
  transition: 0.25s var(--ease);
  display: grid;
  place-items: center;
}

.services__arrow-btn:hover {
  background: var(--blue);
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 12px 24px rgba(38, 159, 243, 0.25);
}

.services__slider {
  position: relative;
  overflow-x: auto;
  overflow-y: visible;
  padding: 20px 0 30px;
  scrollbar-width: none;
  -ms-overflow-style: none;
  cursor: grab;
  touch-action: pan-y;
  scroll-behavior: smooth;
  -webkit-overflow-scrolling: touch;
}

.services__slider::-webkit-scrollbar {
  display: none;
}

.services__slider.is-dragging {
  cursor: grabbing;
  scroll-behavior: auto;
}

.services__track {
  display: flex;
  gap: 28px;
  width: max-content;
  padding-left: calc((100vw - var(--container)) / 2);
  padding-right: calc((100vw - var(--container)) / 2);
}

.services__spacer {
  flex: 0 0 40px;
  min-width: 40px;
}

.service-card {
  flex: 0 0 280px;
  width: 280px;
  background: white;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: var(--shadow-soft);
  transition:
    transform 0.35s var(--ease),
    box-shadow 0.35s var(--ease);
  display: flex;
  flex-direction: column;
}

.service-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 38px rgba(17, 80, 140, 0.18);
}

.service-card__media {
  height: 160px;
  overflow: hidden;
  flex-shrink: 0;
}

.service-card__media img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s var(--ease);
}

.service-card:hover img {
  transform: scale(1.06);
}

.service-card__body {
  padding: 22px 24px 26px;
  display: flex;
  flex-direction: column;
  flex: 1;
}

.service-card__body h3 {
  margin: 0 0 10px;
  font-size: 17px;
  font-weight: 800;
  color: var(--text);
}

.service-card__body p {
  margin: 0 0 12px;
  font-size: 13px;
  line-height: 1.6;
  color: var(--muted);
  flex: 1;
}

.service-card__btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 18px;
  border: 1.5px solid var(--blue);
  border-radius: 6px;
  color: var(--blue-dark);
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  transition: 0.25s var(--ease);
  align-self: flex-start;
}

.service-card__btn:hover {
  background: var(--blue);
  color: white;
  border-color: var(--blue);
}

.service-card__arrow {
  display: inline-block;
  transition: transform 0.25s var(--ease);
}

.service-card__btn:hover .service-card__arrow {
  transform: translateX(4px);
}

.app.is-rtl .service-card__btn:hover .service-card__arrow {
  transform: scaleX(-1) translateX(4px);
}

/* Responsive */
@media (max-width: 992px) {
  .services__track {
    padding-left: 20px;
    padding-right: 20px;
  }

  .services__spacer {
    flex: 0 0 20px;
    min-width: 20px;
  }

  .service-card {
    flex: 0 0 260px;
    width: 260px;
  }

  .services__title-wrapper {
    align-items: flex-start;
  }

  .services__arrows {
    align-self: flex-end;
    margin-top: -10px;
  }
}

@media (max-width: 768px) {
  .services {
    padding-top: 40px;
    padding-bottom: 60px;
  }

  /* ===== اضافه کنید ===== */
  .services__title-wrapper {
    justify-content: center;
  }

  .services__title-icon {
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .services__title-icon h2 {
    display: none; /* مخفی کردن h2 اصلی */
  }

  /* عنوان فارسی */
  .app.is-rtl .services__title-icon::before {
    content: "خدمات ما";
    font-size: 28px;
    font-weight: 900;
    color: var(--blue-dark);
    line-height: 1.15;
  }

  /* عنوان انگلیسی */
  .app:not(.is-rtl) .services__title-icon::before {
    content: "Our Services";
    font-size: 28px;
    font-weight: 900;
    color: var(--blue-dark);
    line-height: 1.15;
  }
  /* ===================== */

  .service-card {
    flex: 0 0 240px;
    width: 240px;
  }

  .services__arrows {
    display: none;
  }
}
@media (max-width: 480px) {
  .service-card {
    flex: 0 0 200px;
    width: 200px;
  }
}

/* ==============================
   8. EXPERTISE
   ============================== */
.expertise {
  padding: 120px 0 160px;
}

.expertise::after {
  content: "";
  position: absolute;
  bottom: -140px;
  width: 250px;
  height: 250px;
  background: #f0f9ff;
  border-radius: 55px;
  transform: rotate(45deg);
}

.expertise__inner {
  display: grid;
  grid-template-columns: 1.05fr 1fr;
  gap: 80px;
  align-items: center;
}

.expertise__visual {
  min-height: 430px;
  position: relative;
}

.expertise-orbit {
  position: relative;
  width: 430px;
  height: 430px;
  margin-left: 95px;
}

.orbit-line {
  position: absolute;
  inset: 45px;
  border: 2px solid rgba(38, 159, 243, 0.12);
  border-radius: 42px;
  transform: rotate(45deg);
  animation: orbitSpin 22s linear infinite;
}

.orbit-line--two {
  inset: 0;
  animation-duration: 32s;
  animation-direction: reverse;
}

@keyframes orbitSpin {
  from {
    transform: rotate(45deg);
  }
  to {
    transform: rotate(405deg);
  }
}

.diamond-title--large {
  position: absolute;
  left: 106px;
  top: 103px;
  width: 225px;
  height: 225px;
  background: linear-gradient(135deg, #1fb2fb, #2089ec);
}

.diamond-title--large::before {
  content: "";
  position: absolute;
  inset: -55px;
  background: rgba(38, 159, 243, 0.15);
  border-radius: 45px;
  z-index: -1;
}

.icon-bubble {
  position: absolute;
  border-radius: 50%;
  display: grid;
  place-items: center;
  color: white;
  font-weight: 900;
  box-shadow: var(--shadow-soft);
  animation: iconFloat 5s ease-in-out infinite;
}

@keyframes iconFloat {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.icon-bubble--camera {
  width: 70px;
  height: 70px;
  background: var(--blue);
  left: -15px;
  top: 94px;
  font-size: 30px;
  animation-delay: 0s;
}

.icon-bubble--play {
  width: 70px;
  height: 70px;
  background: #12c3ce;
  top: 30px;
  left: 192px;
  animation-delay: 0.6s;
}

.icon-bubble--wifi {
  width: 54px;
  height: 54px;
  background: #02bfb8;
  left: 45px;
  bottom: 92px;
  animation-delay: 1.2s;
}

.icon-bubble--star {
  width: 36px;
  height: 36px;
  background: var(--blue);
  right: -4px;
  top: 190px;
  animation-delay: 1.8s;
}

.icon-bubble--lab {
  width: 80px;
  height: 80px;
  background: #681be7;
  right: 55px;
  bottom: 35px;
  font-size: 34px;
  animation-delay: 2.4s;
}

.icon-bubble--small {
  width: 34px;
  height: 34px;
  background: #8647ff;
  left: -48px;
  bottom: 50px;
  animation-delay: 3s;
}

.expertise__content {
  position: relative;
  z-index: 2;
  max-width: 610px;
}

.expertise__content p {
  color: #485363;
  line-height: 1.9;
  margin: 0 0 28px;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.tags span {
  min-width: 104px;
  padding: 9px 16px;
  border: 1px solid #d7dde5;
  color: #9aa3af;
  border-radius: 3px;
  text-align: center;
  font-size: 12px;
  font-weight: 600;
  transition:
    transform 0.25s var(--ease),
    box-shadow 0.25s var(--ease),
    background 0.25s var(--ease),
    color 0.25s var(--ease),
    border-color 0.25s var(--ease);
}

.tags span:hover {
  transform: translateY(-2px);
  border-color: var(--blue);
  color: var(--blue-dark);
  box-shadow: 0 8px 18px rgba(38, 159, 243, 0.14);
}

.tags span.active {
  background: var(--blue);
  color: white;
  border-color: var(--blue);
}

.tags span.active:hover {
  box-shadow: 0 10px 22px rgba(38, 159, 243, 0.3);
}

/* =====================================================
   TESTIMONIALS - COMPACT VERSION
   ===================================================== */
.testimonials {
  position: relative;
  overflow: hidden;
  padding: 70px 0 60px;
  background: #edf8fd;
}

.testimonials::before {
  content: "";
  position: absolute;
  width: 280px;
  height: 350px;
  top: -310px;
  right: -30px;
  background: rgba(255, 255, 255, 0.95);
  transform: rotate(45deg);
  z-index: 0;
  border-radius: 40px;
}

.testimonials__inner {
  position: relative;
  z-index: 1;
  min-height: 380px;
}

.testimonials__title {
  position: absolute;
  z-index: 1;
  top: 50%;
  left: 0;
  width: 240px;
  height: 240px;
  transform: translateY(-50%);
}

.testimonial-diamond {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
  transform: rotate(45deg);
}

.testimonial-diamond h2 {
  margin: 0;
  color: #0d4eae;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 38px;
  font-weight: 500;
  line-height: 1.18;
  white-space: nowrap;
  transform: rotate(-45deg);
}

.app.is-rtl .testimonial-diamond h2 {
  font-family: "Vazirmatn", serif;
}

.testimonial-quote {
  position: absolute;
  top: 18px;
  left: 80px;
  color: #aeeaf4;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 90px;
  font-weight: 700;
  line-height: 0.8;
  transform: rotate(-45deg);
}

.testimonials__cards {
  position: relative;
  z-index: 2;
  width: calc(100% - 130px);
  margin-left: 130px;
  padding: 15px 0 25px;
  overflow-x: auto;
  overflow-y: hidden;
  scrollbar-width: none;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x mandatory;
}

.testimonials__cards::-webkit-scrollbar {
  display: none;
}

.testimonials__track {
  display: flex;
  align-items: flex-start;
  gap: 24px;
  width: max-content;
  min-width: 100%;
  padding: 0 10px 10px;
}

.testimonial-card {
  flex: 0 0 240px;
  scroll-snap-align: start;
}

.testimonial-card__box {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 230px;
  padding: 22px 24px;
  background: #fff;
  border-radius: 45px 45px 45px 0;
  box-shadow: 0 10px 18px rgba(40, 86, 110, 0.08);
  transition:
    transform 0.35s var(--ease),
    box-shadow 0.35s var(--ease);
}

.app.is-rtl .testimonial-card__box {
  border-radius: 45px 45px 0 45px;
}

.testimonial-card:hover .testimonial-card__box {
  transform: translateY(-4px);
  box-shadow: 0 16px 28px rgba(40, 86, 110, 0.12);
}

.testimonial-card__text {
  margin: 0;
  color: #272b30;
  font-size: 13px;
  line-height: 1.55;
}

.testimonial-card__stars {
  display: flex;
  gap: 2px;
  color: #ffb800;
  font-size: 18px;
  line-height: 1;
}

.testimonial-card__stars .is-empty {
  color: #d9dde0;
}

.testimonial-card__client {
  display: flex;
  align-items: center;
  gap: 14px;
  padding-top: 18px;
}

.testimonial-card__avatar {
  width: 56px;
  height: 56px;
  flex: 0 0 56px;
  object-fit: cover;
  border: 3px solid #fff;
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(35, 68, 88, 0.15);
}

.testimonial-card__client h3 {
  margin: 0 0 3px;
  color: #272b30;
  font-size: 17px;
  font-weight: 700;
}

.testimonial-card__client span {
  color: #535a60;
  font-size: 12px;
  font-style: italic;
}

.app.is-rtl .testimonial-card__client span {
  font-style: normal;
}

.testimonial-dots {
  position: relative;
  z-index: 3;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  margin: 4px auto 0;
}

.testimonial-dots button {
  width: 10px;
  height: 10px;
  padding: 0;
  border: 0;
  border-radius: 50%;
  background: #b8eaf2;
  cursor: pointer;
  transition: 0.3s var(--ease);
}

.testimonial-dots button:hover {
  background: #7fd9ea;
}

.testimonial-dots button.active {
  width: 30px;
  border-radius: 20px;
  background: var(--blue);
}

/* =====================================================
   TESTIMONIALS - TABLET
   ===================================================== */
@media (max-width: 1199px) {
  .testimonials {
    padding: 55px 0 50px;
  }

  .testimonials__inner {
    min-height: 340px;
  }

  .testimonials__title {
    width: 190px;
    height: 190px;
  }

  .testimonial-diamond h2 {
    font-size: 30px;
  }

  .testimonial-quote {
    top: 14px;
    left: 25px;
    font-size: 70px;
  }

  .testimonials__cards {
    width: calc(100% - 100px);
    margin-left: 100px;
  }

  .testimonial-card {
    flex-basis: 220px;
  }

  .testimonial-card__box {
    height: 210px;
    padding: 20px 22px;
  }
}

/* =====================================================
   TESTIMONIALS - MOBILE (عنوان در بالا و وسط قرار می‌گیرد)
   ===================================================== */
@media (max-width: 992px) {
  .testimonials {
    padding: 40px 0;
  }

  .testimonials__inner {
    display: flex;
    flex-direction: column;
    min-height: auto;
  }

  .testimonials__title {
    position: relative;
    top: auto;
    left: auto;
    flex: 0 0 auto;
    width: 200px;
    height: 200px;
    margin: 0 auto 20px;
    transform: none;
  }

  .testimonial-diamond h2 {
    font-size: 28px;
    text-align: center;
  }

  .testimonial-quote {
    top: 16px;
    left: 32px;
    font-size: 65px;
  }

  .testimonials__cards {
    width: calc(100% + 24px);
    margin-left: -12px;
    padding: 12px 12px 20px;
  }

  .testimonials__track {
    gap: 16px;
    padding: 0 4px 10px;
  }

  .testimonial-card {
    flex: 0 0 min(280px, 78vw);
  }

  .testimonial-card__box {
    height: 200px;
  }

  .testimonial-dots {
    margin-top: 0;
  }
}

/* =====================================================
   TESTIMONIALS - SMALL MOBILE
   ===================================================== */
@media (max-width: 576px) {
  .testimonials {
    padding: 30px 0 40px;
  }

  .testimonials__title {
    width: 160px;
    height: 160px;
    margin-bottom: 16px;
  }

  .testimonial-diamond h2 {
    font-size: 22px;
  }

  .testimonial-quote {
    top: 12px;
    left: 22px;
    font-size: 50px;
  }

  .testimonial-card {
    flex-basis: 78vw;
  }

  .testimonial-card__box {
    height: 180px;
    padding: 16px 18px;
    border-radius: 35px 35px 35px 0;
  }

  .testimonial-card__text {
    font-size: 12px;
  }

  .testimonial-card__stars {
    font-size: 16px;
  }

  .testimonial-card__client {
    gap: 10px;
    padding-top: 14px;
  }

  .testimonial-card__avatar {
    width: 46px;
    height: 46px;
    flex-basis: 46px;
  }

  .testimonial-card__client h3 {
    font-size: 15px;
  }

  .testimonial-card__client span {
    font-size: 11px;
  }
}

/* ==============================
   10. CASE STUDIES
   ============================== */
.case {
  background: #eef9ff;
  padding: 70px 0 95px;
}

.case__inner {
  display: grid;
  grid-template-columns: 210px 1fr;
  gap: 70px;
}

.case__sidebar h2 {
  color: var(--blue-dark);
  font-size: 42px;
  line-height: 1.05;
  margin: 32px 0 48px;
  font-weight: 900;
}

.case__sidebar ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.case__sidebar li {
  padding: 11px 18px;
  border-radius: 5px;
  color: #263345;
  font-size: 14px;
  margin-bottom: 13px;
  cursor: pointer;
  transition: 0.25s var(--ease);
}

.case__sidebar li:hover {
  background: rgba(38, 159, 243, 0.1);
  color: var(--blue-dark);
}

.case__sidebar li.active {
  background: #b9e5ff;
  color: var(--blue-dark);
}

.case__grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1.45fr;
  grid-auto-rows: 155px;
  gap: 24px;
}

.case-card {
  position: relative;
  overflow: hidden;
  border-radius: 15px;
  background: white;
  box-shadow: var(--shadow-soft);
  transition:
    transform 0.4s var(--ease),
    box-shadow 0.4s var(--ease);
}

.case-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 22px 40px rgba(17, 80, 140, 0.2);
}

.case-card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s var(--ease);
}

.case-card:hover img {
  transform: scale(1.07);
}

.case-card--blue {
  background: linear-gradient(135deg, #19aef8, #1689ed);
}

.case-card--wide {
  grid-column: span 1;
}

.case-card--tall {
  grid-row: span 2;
}

.case-card--logo {
  display: grid;
  place-items: center;
}

.case-logo {
  width: 86px;
  height: 86px;
  display: grid;
  place-items: center;
  background: linear-gradient(135deg, #a050ff, #00bff2);
  color: white;
  font-size: 31px;
  font-weight: 900;
  border-radius: 20px;
  transform: rotate(45deg);
  transition: transform 0.4s var(--ease);
}

.case-card--logo:hover .case-logo {
  transform: rotate(45deg) scale(1.08);
}

.case-card__overlay {
  position: absolute;
  inset: auto 0 0 0;
  padding: 18px;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.65), transparent);
  color: white;
}

.case-card--blue .case-card__overlay {
  inset: 0;
  background: transparent;
  padding: 24px;
}

.case-card__overlay h3 {
  font-size: 16px;
  line-height: 1.28;
  margin: 0;
}

.case-card__overlay p {
  font-size: 11px;
  margin: 5px 0 0;
  opacity: 0.9;
}

.case-icon {
  width: 31px;
  height: 31px;
  background: white;
  color: var(--blue);
  border-radius: 50%;
  display: grid;
  place-items: center;
  margin-bottom: 8px;
  transition: transform 0.35s var(--ease);
}

.case-card:hover .case-icon {
  transform: scale(1.15) rotate(15deg);
}

/* ==============================
   11. CTA
   ============================== */
.cta {
  background: #eef9ff;
  padding: 32px 0 90px;
}

.cta__box {
  background: white;
  border: 2px solid #15bed5;
  border-radius: 16px;
  min-height: 138px;
  padding: 30px 56px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 15px 35px rgba(30, 150, 210, 0.08);
  transition:
    box-shadow 0.35s var(--ease),
    transform 0.35s var(--ease);
}

.cta__box:hover {
  box-shadow: 0 22px 46px rgba(30, 150, 210, 0.16);
  transform: translateY(-3px);
}

.cta h3 {
  margin: 0 0 12px;
  font-size: 24px;
  color: #064264;
}

.cta p {
  margin: 0;
  color: #3b4655;
}

/* ==============================
   FIX: CTA box on mobile – button below text
   ============================== */
@media (max-width: 768px) {
  .cta__box {
    flex-direction: column;
    align-items: stretch;
    padding: 28px 22px;
    gap: 24px;
    text-align: center;
  }

  .cta__box .btn {
    align-self: center;
    width: 100%;
    max-width: 280px;
  }
}

/* ==============================
   12. OFFICE / CONTACT
   ============================== */
.office {
  background: #eef9ff;
  padding: 45px 0 120px;
}

.office::before {
  content: "";
  position: absolute;
  left: 200px;
  top: -10px;
  width: 480px;
  height: 480px;
  background: rgba(255, 255, 255, 0.55);
  border-radius: 62px;
  transform: rotate(45deg);
}

.office__inner {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: 1fr 1.1fr;
  gap: 80px;
  align-items: end;
}

.office-title {
  margin: 0 0 90px 35px;
}

.contact-card {
  max-width: 470px;
  background: white;
  border-radius: 10px;
  padding: 22px 26px;
  margin-bottom: 24px;
  box-shadow: var(--shadow-soft);
  transition:
    transform 0.3s var(--ease),
    box-shadow 0.3s var(--ease);
}

.contact-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 18px 34px rgba(17, 80, 140, 0.16);
}

.contact-card h4 {
  margin: 0 0 16px;
  font-size: 15px;
}

.contact-row {
  display: flex;
  gap: 34px;
  flex-wrap: wrap;
  color: #435163;
  font-size: 12px;
  margin-bottom: 13px;
}

.contact-card p {
  margin: 0;
  color: #435163;
  font-size: 12px;
}

.map {
  position: relative;
  height: 500px;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: var(--shadow-soft);
}

.map img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: saturate(0.75) brightness(1.03);
  transition: transform 0.6s var(--ease);
}

.map:hover img {
  transform: scale(1.05);
}

.map::after {
  content: "";
  position: absolute;
  inset: 0;
  background: rgba(240, 248, 252, 0.18);
}

.map-pin {
  position: absolute;
  left: 47%;
  top: 45%;
  width: 42px;
  height: 42px;
  background: #ff654b;
  border-radius: 50% 50% 50% 0;
  transform: rotate(-45deg);
  z-index: 2;
  box-shadow: 0 8px 18px rgba(255, 101, 75, 0.3);
  animation: pinBounce 2.4s ease-in-out infinite;
}

@keyframes pinBounce {
  0%,
  100% {
    transform: rotate(-45deg) translateY(0);
  }
  50% {
    transform: rotate(-45deg) translateY(-8px);
  }
}

.map-pin::after {
  content: "";
  position: absolute;
  inset: 12px;
  background: white;
  border-radius: 50%;
}

.map-btn {
  position: absolute;
  right: 16px;
  bottom: 16px;
  z-index: 3;
  border: 0;
  background: var(--blue-dark);
  color: white;
  padding: 11px 18px;
  border-radius: 6px;
  font-weight: 800;
  font-size: 12px;
  cursor: pointer;
  transition:
    transform 0.3s var(--ease),
    background 0.3s var(--ease);
}

.app.is-rtl .map-btn {
  right: auto;
  left: 16px;
}

.map-btn:hover {
  transform: translateY(-2px);
  background: var(--blue);
}

/* ==============================
   13. FOOTER
   ============================== */
.footer {
  background: white;
  padding: 64px 0 42px;
}

.footer__inner {
  display: grid;
  grid-template-columns: 1.5fr 1fr 1fr 1fr 1.1fr;
  gap: 48px;
}

.brand--footer .brand__mark {
  width: 34px;
  height: 34px;
  border-radius: 9px;
  background: #d7dce2;
  box-shadow: none;
}

.brand--footer .brand__mark::after {
  content: "RP";
  font-size: 13px;
}

.brand--footer .brand__text {
  color: #a4acb7;
  font-size: 19px;
}

.footer__brand p {
  color: #a1a9b4;
  line-height: 1.7;
  max-width: 310px;
  font-size: 13px;
}

.footer__brand small {
  color: #a1a9b4;
  font-size: 12px;
}

.footer-col h4 {
  color: var(--blue-dark);
  font-size: 12px;
  margin: 0 0 22px;
  font-weight: 900;
}

.footer-col a {
  display: block;
  color: #8c96a4;
  font-size: 13px;
  margin-bottom: 14px;
  transition:
    color 0.25s var(--ease),
    transform 0.25s var(--ease);
}

.footer-col a:hover {
  color: var(--blue-dark);
  transform: translateX(3px);
}

.app.is-rtl .footer-col a:hover {
  transform: translateX(-3px);
}

.socials {
  display: flex;
  gap: 16px;
  margin-bottom: 32px;
}

.socials a {
  width: 28px;
  height: 28px;
  display: grid;
  place-items: center;
  color: var(--blue-dark);
  background: #edf7ff;
  border-radius: 50%;
  font-size: 12px;
  font-weight: 900;
  transition:
    transform 0.3s var(--ease),
    background 0.3s var(--ease),
    color 0.3s var(--ease);
}

.socials a:hover {
  background: var(--blue);
  color: white;
  transform: translateY(-3px);
}

.footer-social select {
  width: 210px;
  height: 42px;
  border: 1px solid #d7e4ef;
  border-radius: 8px;
  color: var(--blue-dark);
  padding: 0 14px;
  background: white;
  cursor: pointer;
  transition: border-color 0.25s ease;
}

.footer-social select:hover {
  border-color: var(--blue);
}

/* ==============================
   14. BACK TO TOP BUTTON
   ============================== */
.to-top {
  position: fixed;
  right: 30px;
  bottom: 28px;
  width: 62px;
  height: 62px;
  border: 0;
  border-radius: 50%;
  background: #9ee7ff;
  color: var(--blue-dark);
  font-size: 34px;
  cursor: pointer;
  z-index: 99;
  opacity: 0;
  visibility: hidden;
  transform: translateY(20px);
  box-shadow: 0 12px 28px rgba(22, 130, 220, 0.2);
  transition: 0.3s var(--ease);
}

.app.is-rtl .to-top {
  right: auto;
  left: 30px;
}

.to-top.show {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.to-top:hover {
  background: var(--blue);
  color: white;
  transform: translateY(-4px);
}

/* ==============================
   15. RESPONSIVE – GENERAL
   ============================== */
@media (max-width: 1200px) {
  .hero__image {
    width: 78vw;
    right: -135px;
  }

  .case__grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 992px) {
  .container {
    width: min(100% - 34px, var(--container));
  }

  .setting-btn {
    display: none;
  }

  .menu-btn {
    display: flex;
  }

  .nav {
    position: fixed;
    top: 76px;
    left: 18px;
    right: 18px;
    background: rgba(255, 255, 255, 0.98);
    border-radius: 18px;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    padding: 16px;
    box-shadow: var(--shadow);
    opacity: 0;
    pointer-events: none;
    transform: translateY(-15px);
    transition: 0.25s;
  }

  .nav.active {
    opacity: 1;
    pointer-events: auto;
    transform: translateY(0);
  }

  .nav-language {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    min-height: 46px;
    margin: 4px 0 0;
    padding: 12px 10px;
    border: 0;
    border-top: 1px solid #edf1f4;
    background: transparent;
    color: #1f3447;
    font: inherit;
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
  }

  .nav-language span:first-child {
    display: inline-grid;
    place-items: center;
    min-width: 36px;
    height: 30px;
    padding: 0 8px;
    border-radius: 8px;
    background: #eef8ff;
    color: var(--blue-dark);
    font-size: 13px;
    font-weight: 900;
  }

  .nav-language span:last-child {
    margin-inline-start: 12px;
  }

  .app.is-rtl .nav-language span:first-child {
    order: 2;
  }

  .app.is-rtl .nav-language span:last-child {
    margin-inline-start: 0;
    margin-inline-end: 12px;
  }

  .nav-language:hover {
    color: var(--blue-dark);
    background: rgba(38, 159, 243, 0.05);
  }

  .nav a,
  .header.scrolled .nav a {
    color: #1f3447;
    padding: 15px 12px;
    border-bottom: 1px solid #eef3f6;
  }

  .hero {
    min-height: 850px;
  }

  .hero__image {
    width: 95vw;
    height: 560px;
    right: -180px;
    top: -90px;
    border-width: 30px;
    border-radius: 0 0 105px 105px;
    border-bottom-left-radius: 105px;
    border-bottom-right-radius: 105px;
  }

  .hero__content {
    align-items: flex-end;
    padding-bottom: 80px;
  }

  .hero__text {
    margin-top: 0;
  }

  .about__inner,
  .expertise__inner,
  .testimonials__inner,
  .office__inner {
    grid-template-columns: 1fr;
  }

  .about__visual {
    order: 1;
  }

  .about__content {
    order: 2;
    margin-inline: auto;
    text-align: center;
  }

  .stats {
    justify-content: center;
  }

  blockquote {
    text-align: left;
  }

  .app.is-rtl blockquote {
    text-align: right;
  }

  .expertise-orbit {
    margin-inline: auto;
  }

  .case__inner {
    grid-template-columns: 1fr;
    gap: 30px;
  }

  .case__sidebar {
    text-align: center;
  }

  .case__sidebar ul {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 10px;
  }

  .case__sidebar li {
    margin: 0;
  }

  .footer__inner {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* ==============================
   16. HERO DESKTOP FIX (≥1170px)
   ============================== */
@media (min-width: 1170px) {
  .hero__image {
    width: 88vw !important;
    max-width: none !important;
    height: 760px !important;
    top: -145px !important;
    right: -360px !important;
    box-shadow: none !important;
  }

  .hero__image img {
    width: 145% !important;
    height: 145% !important;
    filter: none !important;
  }

  .hero::before,
  .hero::after {
    display: none !important;
  }
}

/* ==============================
   17. TABLET LAYOUT (769px – 1169px)
   ============================== */
@media (min-width: 769px) and (max-width: 1169px) {
  body,
  .app {
    overflow-x: hidden;
  }

  .header,
  .header.scrolled {
    position: fixed;
    inset: 0 0 auto 0;
    z-index: 130;
    width: 100%;
    height: 0;
    background: transparent !important;
    box-shadow: none !important;
    backdrop-filter: none !important;
  }

  .header__inner {
    position: static;
    width: 100%;
    height: 0;
    margin: 0;
    padding: 0;
  }

  .header__inner .brand {
    position: fixed !important;
    top: 22px !important;
    left: 28px !important;
    right: auto !important;
    z-index: 135 !important;
    display: inline-flex !important;
    align-items: center !important;
    gap: 10px !important;
    margin: 0 !important;
    padding: 0 !important;
    color: #ffffff !important;
    text-decoration: none !important;
    width: auto !important;
    height: 42px !important;
    white-space: nowrap !important;
    animation: fadeDown 0.7s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .header > .brand {
    display: none !important;
  }

  .brand .brand__text ~ .brand__text {
    display: none !important;
  }

  .brand__mark {
    display: block !important;
    flex: 0 0 40px !important;
    width: 40px !important;
    height: 40px !important;
  }

  .brand__text {
    display: block !important;
    width: auto !important;
    margin: 0 !important;
    padding: 0 !important;
    color: #ffffff !important;
    font-size: 27px;
    font-weight: 800;
    line-height: 1 !important;
    text-shadow: 0 2px 7px rgba(0, 0, 0, 0.28);
    white-space: nowrap !important;
  }

  .setting-btn {
    display: none !important;
  }

  .menu-btn {
    position: fixed;
    top: 20px;
    right: 24px;
    left: auto;
    z-index: 140;
    display: flex !important;
    width: 45px;
    height: 45px;
    margin: 0;
    padding: 0;
    flex-direction: column;
    align-items: flex-end;
    justify-content: center;
    gap: 6px;
    color: #111111 !important;
    border: 0;
    background: transparent;
    cursor: pointer;
    animation: fadeDown 0.7s 0.1s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .menu-btn span {
    display: block;
    width: 30px;
    height: 3px;
    border-radius: 20px;
    background: #111111 !important;
    box-shadow: none !important;
    transition:
      transform 0.25s ease,
      opacity 0.2s ease;
  }

  .menu-btn.active span:nth-child(1) {
    transform: translateY(9px) rotate(45deg);
  }
  .menu-btn.active span:nth-child(2) {
    opacity: 0;
  }
  .menu-btn.active span:nth-child(3) {
    transform: translateY(-9px) rotate(-45deg);
  }

  .nav {
    position: fixed;
    top: 78px;
    right: 20px;
    left: auto;
    z-index: 135;
    display: flex !important;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    width: min(330px, calc(100vw - 40px));
    margin: 0;
    padding: 12px 16px;
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    background: rgba(255, 255, 255, 0.98);
    border-radius: 17px;
    box-shadow: 0 14px 38px rgba(11, 26, 40, 0.2);
    transform: translateY(-12px) scale(0.98);
    transform-origin: top right;
    transition:
      opacity 0.2s ease,
      transform 0.2s ease,
      visibility 0s linear 0.2s;
    will-change: opacity, transform;
  }

  .nav.active {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
    transform: translateY(0) scale(1);
    transition:
      opacity 0.2s ease,
      transform 0.2s ease,
      visibility 0s linear 0s;
  }

  .nav a,
  .header.scrolled .nav a {
    display: block;
    padding: 14px 10px;
    color: #1f3447 !important;
    font-size: 14px;
    font-weight: 700;
    text-align: right;
    text-decoration: none;
    border-bottom: 1px solid #edf1f4;
  }

  .nav a:last-child {
    border-bottom: 0;
  }

  .nav-language {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    min-height: 46px;
    margin: 4px 0 0;
    padding: 12px 10px;
    border: 0;
    border-top: 1px solid #edf1f4;
    background: transparent;
    color: #1f3447;
    font: inherit;
    font-size: 14px;
    font-weight: 700;
    text-align: right;
    cursor: pointer;
  }

  .nav-language span:first-child {
    display: inline-grid;
    place-items: center;
    min-width: 36px;
    height: 30px;
    padding: 0 8px;
    border-radius: 8px;
    background: #eef8ff;
    color: var(--blue-dark);
    font-size: 13px;
    font-weight: 900;
  }

  .nav-language span:last-child {
    margin-inline-start: 12px;
  }

  .app.is-rtl .nav-language {
    text-align: left;
  }

  .app.is-rtl .nav-language span:first-child {
    order: 2;
  }

  .app.is-rtl .nav-language span:last-child {
    margin-inline-start: 0;
    margin-inline-end: 12px;
  }

  .nav-language:hover {
    color: var(--blue-dark);
    background: rgba(38, 159, 243, 0.05);
  }

  .hero {
    height: 705px;
    min-height: 705px;
    padding: 0;
    overflow: hidden;
    background: #ffffff;
  }

  .hero::before,
  .hero::after {
    display: none !important;
  }

  .hero__image {
    position: absolute !important;
    top: 0 !important;
    left: 0 !important;
    right: auto !important;
    width: calc(100% - 48px) !important;
    max-width: none !important;
    height: 705px !important;
    padding: 0 !important;
    overflow: hidden !important;
    background: #f41561;
    border: 0 !important;
    border-radius: 0 !important;
    box-shadow: none !important;
    transform: none !important;
    clip-path: polygon(
      0 0,
      100% 0,
      calc(100% - 175px) 100%,
      0 calc(100% - 205px)
    );
    z-index: 0;
    animation: slideDown 0.9s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .hero__image img {
    position: absolute;
    inset: 0 31px 25px 0;
    width: calc(100% - 31px) !important;
    height: calc(100% - 25px) !important;
    max-width: none !important;
    object-fit: cover;
    object-position: center center;
    filter: brightness(0.52) saturate(0.8) !important;
    transform: none !important;
    clip-path: polygon(
      0 0,
      100% 0,
      calc(100% - 152px) 100%,
      0 calc(100% - 180px)
    );
  }

  .hero__content {
    position: relative;
    z-index: 2;
    display: flex;
    align-items: center;
    justify-content: center;
    width: min(650px, calc(100% - 145px));
    min-height: 620px;
    height: 620px;
    margin: 0 auto;
    padding: 82px 0 0;
    text-align: center;
    animation: fadeUp 0.8s 0.2s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .hero__text {
    width: 100%;
    margin: 18px 0 0;
  }

  .hero h1 {
    margin: 0 0 42px;
    color: #ffffff;
    font-size: clamp(38px, 5.1vw, 52px);
    font-weight: 900;
    line-height: 1.12;
    letter-spacing: -1.6px;
    text-shadow: 0 2px 7px rgba(0, 0, 0, 0.28);
  }

  .hero p {
    max-width: 620px;
    margin: 0 auto 34px;
    color: #ffffff;
    font-size: clamp(20px, 2.8vw, 29px);
    font-weight: 700;
    line-height: 1.48;
    text-shadow: 0 2px 6px rgba(0, 0, 0, 0.25);
  }

  .hero .btn--primary {
    min-width: 250px;
    min-height: 60px;
    padding: 0 28px;
    color: #ffffff;
    font-size: 15px;
    background: rgba(20, 31, 41, 0.38);
    border: 1px solid rgba(255, 255, 255, 0.72);
    border-radius: 6px;
    box-shadow: none;
  }

  @keyframes fadeDown {
    from {
      opacity: 0;
      transform: translateY(-25px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes slideDown {
    from {
      opacity: 0;
      transform: translateY(-60px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes fadeUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}

/* ==============================
   18. MOBILE LAYOUT (≤768px)
   ============================== */
@media (max-width: 768px) {
  body,
  .app {
    overflow-x: hidden;
  }

  .header,
  .header.scrolled {
    position: fixed !important;
    inset: 0 0 auto 0 !important;
    z-index: 130 !important;
    width: 100% !important;
    height: 0 !important;
    background: transparent !important;
    box-shadow: none !important;
    backdrop-filter: none !important;
  }

  .header__inner {
    position: static !important;
    width: 100% !important;
    height: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
  }

  .menu-btn {
    position: fixed !important;
    top: 82px !important;
    left: 15px !important;
    right: auto !important;
    z-index: 145 !important;
    display: flex !important;
    flex-direction: column !important;
    align-items: flex-start !important;
    justify-content: center !important;
    gap: 6px !important;
    width: 42px !important;
    height: 42px !important;
    margin: 0 !important;
    padding: 0 4px !important;
    background: transparent !important;
    border: 0 !important;
    cursor: pointer !important;
    animation: fadeDown 0.7s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .menu-btn span {
    display: block !important;
    width: 29px !important;
    height: 3px !important;
    border-radius: 20px !important;
    background: #ffffff !important;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.25) !important;
    transition:
      transform 0.25s ease,
      opacity 0.2s ease !important;
  }

  .menu-btn.active span:nth-child(1) {
    transform: translateY(9px) rotate(45deg);
  }

  .menu-btn.active span:nth-child(2) {
    opacity: 0;
  }

  .menu-btn.active span:nth-child(3) {
    transform: translateY(-9px) rotate(-45deg);
  }

  .header__inner .brand {
    position: fixed !important;
    top: 78px !important;
    left: 76px !important;
    right: auto !important;
    z-index: 140 !important;
    display: inline-flex !important;
    align-items: center !important;
    gap: 10px !important;
    width: auto !important;
    height: 48px !important;
    margin: 0 !important;
    padding: 0 !important;
    color: #ffffff !important;
    text-decoration: none !important;
    white-space: nowrap !important;
    animation: fadeDown 0.7s 0.08s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .header > .brand {
    display: none !important;
  }

  .brand .brand__text ~ .brand__text {
    display: none !important;
  }

  .brand__mark {
    display: grid !important;
    flex: 0 0 42px !important;
    width: 42px !important;
    height: 42px !important;
    border-radius: 11px !important;
  }

  .brand__mark::after {
    font-size: 16px !important;
  }

  .brand__text {
    display: block !important;
    width: auto !important;
    margin: 0 !important;
    padding: 0 !important;
    color: #ffffff !important;
    font-size: 26px !important;
    font-weight: 800 !important;
    line-height: 1 !important;
    white-space: nowrap !important;
    text-shadow: 0 2px 6px rgba(0, 0, 0, 0.28) !important;
  }

  .setting-btn {
    position: fixed !important;
    top: 83px !important;
    right: 16px !important;
    left: auto !important;
    z-index: 145 !important;
    display: grid !important;
    place-items: center !important;
    width: 42px !important;
    height: 42px !important;
    margin: 0 !important;
    padding: 0 !important;
    color: #ffffff !important;
    font-size: 28px !important;
    background: transparent !important;
    border: 0 !important;
    border-radius: 0 !important;
    box-shadow: none !important;
    animation: fadeDown 0.7s 0.16s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .setting-btn svg {
    fill: #ffffff !important;
    stroke: #ffffff !important;
    filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.22));
  }

  .nav {
    position: fixed !important;
    top: 132px !important;
    left: 14px !important;
    right: auto !important;
    z-index: 135 !important;
    display: flex !important;
    flex-direction: column !important;
    align-items: stretch !important;
    gap: 0 !important;
    width: min(310px, calc(100vw - 28px)) !important;
    margin: 0 !important;
    padding: 12px 16px !important;
    visibility: hidden !important;
    opacity: 0 !important;
    pointer-events: none !important;
    background: rgba(255, 255, 255, 0.98) !important;
    border-radius: 17px !important;
    box-shadow: 0 14px 38px rgba(11, 26, 40, 0.22) !important;
    transform: translateY(-14px) scale(0.98) !important;
    transform-origin: top left !important;
    transition:
      opacity 0.2s ease,
      transform 0.2s ease,
      visibility 0s linear 0.2s !important;
  }

  .nav.active {
    visibility: visible !important;
    opacity: 1 !important;
    pointer-events: auto !important;
    transform: translateY(0) scale(1) !important;
    transition:
      opacity 0.2s ease,
      transform 0.2s ease,
      visibility 0s linear 0s !important;
  }

  .nav a,
  .header.scrolled .nav a {
    display: block !important;
    padding: 14px 10px !important;
    color: #1f3447 !important;
    font-size: 14px !important;
    font-weight: 700 !important;
    text-align: left !important;
    border-bottom: 1px solid #edf1f4 !important;
  }

  .nav a:last-child {
    border-bottom: 0 !important;
  }

  .hero {
    position: relative !important;
    height: 665px !important;
    min-height: 665px !important;
    padding: 0 !important;
    overflow: hidden !important;
    background: #1a3542 !important;
  }

  .hero::before,
  .hero::after {
    display: none !important;
  }

  .hero__image {
    position: absolute !important;
    inset: 0 !important;
    width: 100% !important;
    max-width: none !important;
    height: 665px !important;
    margin: 0 !important;
    padding: 0 !important;
    overflow: hidden !important;
    background: #1a3542 !important;
    border: 0 !important;
    border-radius: 0 !important;
    box-shadow: none !important;
    clip-path: none !important;
    transform: none !important;
    z-index: 0 !important;
    animation: slideDown 0.9s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .hero__image::before {
    content: "" !important;
    position: absolute !important;
    inset: 0 !important;
    z-index: 1 !important;
    display: block !important;
    background: rgba(7, 29, 39, 0.43) !important;
  }

  .hero__image img {
    position: absolute !important;
    inset: 0 !important;
    width: 100% !important;
    height: 100% !important;
    max-width: none !important;
    object-fit: cover !important;
    object-position: center center !important;
    filter: none !important;
    clip-path: none !important;
    transform: none !important;
  }

  .hero__content {
    position: relative !important;
    z-index: 2 !important;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    width: min(100% - 42px, 520px) !important;
    height: 665px !important;
    min-height: 665px !important;
    margin: 0 auto !important;
    padding: 150px 0 55px !important;
    text-align: center !important;
    animation: fadeUp 0.8s 0.24s cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .hero__text {
    width: 100% !important;
    margin: 0 !important;
  }

  .hero h1 {
    margin: 0 0 38px !important;
    color: #ffffff !important;
    font-size: clamp(29px, 7.4vw, 39px) !important;
    font-weight: 900 !important;
    line-height: 1.15 !important;
    letter-spacing: -1px !important;
    text-shadow: 0 2px 7px rgba(0, 0, 0, 0.3) !important;
  }

  .hero p {
    max-width: 450px !important;
    margin: 0 auto 34px !important;
    color: #ffffff !important;
    font-size: clamp(18px, 4.8vw, 24px) !important;
    font-weight: 700 !important;
    line-height: 1.48 !important;
    text-shadow: 0 2px 6px rgba(0, 0, 0, 0.28) !important;
  }

  .hero .btn--primary {
    min-width: 250px !important;
    min-height: 60px !important;
    padding: 0 25px !important;
    color: #ffffff !important;
    font-size: 15px !important;
    font-weight: 900 !important;
    background: rgba(22, 35, 43, 0.28) !important;
    border: 1px solid rgba(255, 255, 255, 0.75) !important;
    border-radius: 6px !important;
    box-shadow: none !important;
  }

  .hero .btn--primary:hover {
    transform: translateY(-3px) !important;
    background: rgba(22, 35, 43, 0.48) !important;
    box-shadow: none !important;
  }

  @keyframes fadeDown {
    from {
      opacity: 0;
      transform: translateY(-25px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes slideDown {
    from {
      opacity: 0;
      transform: translateY(-60px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes fadeUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}

/* ==============================
   19. HEADER SCROLL FIX – TABLET
   ============================== */
@media (min-width: 769px) and (max-width: 1169px) {
  .header,
  .header.scrolled {
    height: 76px !important;
    transition:
      background 0.3s ease,
      box-shadow 0.3s ease,
      backdrop-filter 0.3s ease !important;
  }

  .header:not(.scrolled) {
    background: transparent !important;
    box-shadow: none !important;
    backdrop-filter: none !important;
  }

  .header.scrolled {
    background: rgba(255, 255, 255, 0.94) !important;
    box-shadow: 0 10px 30px rgba(20, 95, 160, 0.1) !important;
    backdrop-filter: blur(16px) !important;
  }

  .header__inner {
    height: 76px !important;
  }

  .header.scrolled .brand__text {
    color: #2a2c32 !important;
    text-shadow: none !important;
  }

  .header.scrolled .menu-btn span {
    background: #111111 !important;
  }
}

/* ==============================
   20. HEADER SCROLL FIX – MOBILE
   ============================== */
@media (max-width: 768px) {
  .header,
  .header.scrolled {
    height: 70px !important;
    transition:
      background 0.3s ease,
      box-shadow 0.3s ease,
      backdrop-filter 0.3s ease !important;
  }

  .header:not(.scrolled) {
    background: transparent !important;
    box-shadow: none !important;
    backdrop-filter: none !important;
  }

  .header.scrolled {
    background: rgba(255, 255, 255, 0.95) !important;
    box-shadow: 0 8px 25px rgba(20, 95, 160, 0.1) !important;
    backdrop-filter: blur(16px) !important;
  }

  .header__inner {
    height: 70px !important;
  }

  .menu-btn {
    top: 14px !important;
    left: 14px !important;
  }

  .header__inner .brand {
    top: 14px !important;
    left: 70px !important;
    height: 42px !important;
  }

  .setting-btn {
    top: 14px !important;
    right: 14px !important;
  }

  .nav {
    top: 70px !important;
  }

  .header.scrolled .brand__text {
    color: #2a2c32 !important;
    text-shadow: none !important;
  }

  .header.scrolled .menu-btn span {
    background: #111111 !important;
    box-shadow: none !important;
  }

  .header.scrolled .setting-btn {
    color: var(--blue-dark) !important;
  }

  .header.scrolled .setting-btn svg {
    fill: var(--blue-dark) !important;
    stroke: var(--blue-dark) !important;
    filter: none !important;
  }

  .header:not(.scrolled) .brand__text {
    color: #ffffff !important;
  }

  .header:not(.scrolled) .menu-btn span {
    background: #ffffff !important;
  }

  .header:not(.scrolled) .setting-btn {
    color: #ffffff !important;
  }
}

/* ==============================
   21. EXTRA SMALL MOBILE
   ============================== */
@media (max-width: 380px) {
  .header__inner .brand {
    left: 64px !important;
  }

  .brand__mark {
    width: 38px !important;
    height: 38px !important;
    flex-basis: 38px !important;
  }

  .brand__text {
    font-size: 23px !important;
  }
}

/* =========================================================
   ABOUT – FIX OVERFLOW ON MOBILE
   ========================================================= */
@media (max-width: 768px) {
  .about {
    overflow: hidden;
    padding: 40px 0 80px;
  }

  .about__inner {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
    min-height: auto;
  }

  .about__visual {
    width: 100%;
    height: 320px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  .about-card {
    position: relative;
    left: auto !important;
    top: auto !important;
    width: 280px;
    height: 220px;
    margin: 0 auto;
  }

  .diamond-title {
    width: 130px;
    height: 130px;
    border-radius: 22px;
  }

  .diamond-title span {
    font-size: 24px;
  }

  .diamond-image {
    position: absolute;
    top: -18px;
    left: 90px;
    width: 150px;
    height: 150px;
    border-radius: 24px;
  }

  .diamond-image img {
    width: 160%;
    height: 195%;
    transform: rotate(-45deg) translate(45px, -40px);
  }

  .soft-diamond--one {
    width: 180px;
    height: 180px;
    left: -30px;
    top: 70px;
  }

  .soft-diamond--two {
    display: none;
  }

  .about__content {
    max-width: 100%;
    padding-top: 0;
    text-align: center;
  }

  .about__content h2 {
    max-width: 100%;
    font-size: 22px;
  }

  .stats {
    gap: 30px;
    justify-content: center;
    flex-wrap: wrap;
  }

  .stat strong {
    font-size: 26px;
  }

  .stat strong::after {
    width: 30px;
    height: 4px;
  }

  blockquote {
    font-size: 18px;
    padding-left: 16px;
    text-align: left;
  }

  .app.is-rtl blockquote {
    padding-left: 0;
    padding-right: 16px;
    text-align: right;
  }

  /* RTL adjustments for about on mobile */

  .app.is-rtl .diamond-image {
    left: auto !important;
    right: 90px !important;
  }

  .app.is-rtl .soft-diamond--one {
    left: auto !important;
    right: -30px !important;
  }
}

@media (max-width: 480px) {
  .about-card {
    height: 180px;
  }

  .diamond-title {
    width: 105px;
    height: 105px;
    border-radius: 18px;
  }

  .diamond-title span {
    font-size: 20px;
  }

  .diamond-image {
    top: -14px;
    left: 70px;
    width: 120px;
    height: 120px;
    border-radius: 20px;
  }

  .diamond-image img {
    transform: rotate(-45deg) translate(60px, -32px);
  }

  .soft-diamond--one {
    width: 140px;
    height: 140px;
    left: -20px;
    top: 50px;
  }

  /* =========================
     نسخه فارسی
     ========================= */

  .app.is-rtl .diamond-image {
    right: 70px !important;
    left: auto !important;

    /* فقط جابه‌جایی افقی */
    translate: 70px 0;
  }

  .app.is-rtl .soft-diamond--one {
    right: -20px !important;
    left: auto !important;
  }
}

/* =========================================================
   EXPERTISE – TABLET / MOBILE DESIGN (below 992px)
   ========================================================= */
.expertise__responsive-title {
  display: none;
}

@media (max-width: 1168px) {
  .expertise {
    position: relative;
    overflow: hidden;
    padding: 58px 0 95px;
    background: #ffffff;
  }

  .expertise::after {
    content: "";
    position: absolute;
    z-index: 0;
    width: 650px;
    height: 650px;
    left: 50%;
    top: 155px;
    right: auto;
    bottom: auto;
    background: #f0f8ff;
    border-radius: 58px;
    transform: translateX(-50%) rotate(45deg);
  }

  .expertise__inner {
    position: relative;
    z-index: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0;
  }

  .expertise__visual {
    display: none;
  }

  .expertise__responsive-title {
    display: inline-block;
    margin: 0 0 16px;
    padding: 5px 10px 7px;
    color: #3164d4;
    font-family: Georgia, "Times New Roman", serif;
    font-size: clamp(30px, 5vw, 48px);
    font-weight: 700;
    line-height: 1;
    text-align: center;
  }

  .app.is-rtl .expertise__responsive-title {
    font-family: "Vazirmatn", serif;
  }

  .expertise__content {
    position: relative;
    z-index: 2;
    width: 100%;
    max-width: 1050px;
    padding: 0;
    text-align: center;
  }

  .expertise__content h2 {
    max-width: 850px;
    margin: 0 auto 68px;
    color: #252525;
    font-size: clamp(25px, 3.2vw, 34px);
    line-height: 1.4;
    letter-spacing: -0.7px;
  }

  .expertise__content p {
    max-width: 1030px;
    margin: 0 auto 42px;
    color: #292d32;
    font-size: clamp(16px, 2vw, 20px);
    font-weight: 400;
    line-height: 2;
  }

  .tags {
    max-width: 540px;
    margin: 0 auto;
    justify-content: center;
    gap: 12px 10px;
  }

  .tags span {
    min-width: auto;
    padding: 8px 10px;
    color: #2d3339;
    border: 1px solid #cfd8e0;
    border-radius: 6px;
    background: rgba(255, 255, 255, 0.35);
    font-size: 14px;
  }

  .tags span.active {
    color: #ffffff;
    background: #2196ed;
    border-color: #2196ed;
  }
}

@media (max-width: 576px) {
  .expertise {
    padding: 45px 0 75px;
  }

  .expertise::after {
    width: 430px;
    height: 430px;
    top: 180px;
    border-radius: 42px;
  }

  .expertise__responsive-title {
    margin-bottom: 22px;
    font-size: 30px;
    padding: 5px 8px 7px;
  }

  .expertise__content h2 {
    margin-bottom: 32px;
    font-size: 23px;
    line-height: 1.45;
  }

  .expertise__content p {
    margin-bottom: 30px;
    font-size: 15px;
    line-height: 1.85;
  }

  .tags {
    max-width: 340px;
    gap: 10px 8px;
  }

  .tags span {
    padding: 8px 9px;
    font-size: 12px;
  }
}

/* =========================================================
   TESTIMONIALS - مانند Our Services (overrides for compact)
   ========================================================= */
.testimonials {
  position: relative;
  overflow: hidden;
}

.testimonials__inner {
  position: relative;
  min-height: 380px;
}

.testimonials__title {
  position: absolute;
  top: 50%;
  left: 0;
  z-index: 1;
  pointer-events: none;
  transform: translateY(-50%);
}

.testimonial-diamond {
  position: relative;
  width: 230px;
  height: 230px;
  display: grid;
  place-items: center;
  transform: rotate(45deg);
  background: #f0f9ff;
}

.testimonial-diamond h2,
.testimonial-quote {
  transform: rotate(-45deg);
}

.testimonial-diamond h2 {
  position: relative;
  z-index: 2;
  margin: 0;
  color: #222;
  font-size: 28px;
  line-height: 1.15;
  font-weight: 700;
}

.testimonial-quote {
  position: absolute;
  top: 55px;
  right: 200px;
  color: #ff5a3d;
  font-size: 70px;
  font-family: Georgia, serif;
  line-height: 1;
}

.testimonials__cards {
  position: relative;
  z-index: 2;
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  padding: 20px 0 30px;
  scrollbar-width: none;
  -ms-overflow-style: none;
  cursor: grab;
  user-select: none;
  touch-action: pan-y;
  -webkit-user-select: none;
  -webkit-overflow-scrolling: touch;
}

.testimonials__cards::-webkit-scrollbar {
  display: none;
}

.testimonials__cards.is-dragging {
  cursor: grabbing;
  scroll-behavior: auto !important;
}

.testimonials__track {
  display: flex;
  align-items: stretch;
  gap: 20px;
  width: max-content;
  padding-left: clamp(180px, 22vw, 300px);
  padding-right: 30px;
}

.testimonial-card {
  flex: 0 0 clamp(230px, 24vw, 300px);
  min-width: 0;
}

.testimonial-card img {
  pointer-events: none;
  -webkit-user-drag: none;
  user-select: none;
}

.testimonial-dots {
  position: relative;
  z-index: 3;
}

@media (max-width: 991px) {
  .testimonials__inner {
    min-height: auto;
  }

  .testimonials__title {
    top: -20px;
    left: 18px;
    transform: none;
  }

  .testimonial-diamond {
    width: 160px;
    height: 120px;
  }

  .testimonial-diamond h2 {
    font-size: 20px;
  }

  .testimonial-quote {
    top: -10px;
    right: 18px;
    font-size: 48px;
  }

  .testimonials__cards {
    padding-top: 16px;
    padding-bottom: 22px;
  }

  .testimonials__track {
    gap: 16px;
    padding-left: 150px;
    padding-right: 16px;
  }

  .testimonial-card {
    flex-basis: min(68vw, 300px);
  }
}

@media (max-width: 577px) {
  .testimonials__title {
    top: -4px;
    left: 12px;
  }

  .testimonial-diamond {
    width: 120px;
    height: 120px;
  }

  .testimonial-diamond h2 {
    font-size: 15px;
  }

  .testimonial-quote {
    top: 10px;
    right: 12px;
    font-size: 36px;
  }

  .testimonials__track {
    gap: 14px;
    padding-left: 100px;
    padding-right: 14px;
  }

  .testimonial-card {
    flex: 0 0 calc(100vw - 60px);
  }
}

.testimonials__cards {
  cursor: grab;
  user-select: none;
  -webkit-user-select: none;
  touch-action: pan-y;
  overscroll-behavior-x: contain;
}

.testimonials__cards.is-dragging {
  cursor: grabbing;
  scroll-behavior: auto !important;
}

.testimonials__track,
.testimonial-card {
  user-select: none;
  -webkit-user-select: none;
}

.testimonial-card img,
.testimonial-card__avatar {
  pointer-events: none;
  user-select: none;
  -webkit-user-drag: none;
}

/* =========================================
   TESTIMONIALS – SMOOTH DRAG / NO JUMP
   ========================================= */
.testimonials__cards {
  scroll-snap-type: none !important;
  scroll-behavior: auto !important;
  cursor: grab;
  user-select: none;
  -webkit-user-select: none;
  overflow-x: auto;
  overflow-y: hidden;
  touch-action: pan-y;
  overscroll-behavior-x: contain;
}

.testimonials__cards.is-dragging {
  cursor: grabbing;
  scroll-snap-type: none !important;
  scroll-behavior: auto !important;
}

.testimonials__cards.is-dragging,
.testimonials__cards.is-dragging * {
  user-select: none;
  -webkit-user-select: none;
}

.testimonial-card,
.testimonial-card * {
  user-select: none;
  -webkit-user-select: none;
}

.testimonial-card img,
.testimonial-card__avatar {
  pointer-events: none;
  -webkit-user-drag: none;
}

/* =====================================================
   TESTIMONIALS – کارت‌ها کامل روی Client Testimonials بیایند
   ===================================================== */
.testimonials {
  overflow: hidden;
}

.testimonials__cards {
  overflow-x: auto !important;
  overflow-y: visible !important;
  width: calc(100% + 40px) !important;
  margin-left: -20px !important;
  padding: 20px 20px 35px !important;
  scroll-snap-type: none !important;
  scroll-behavior: auto !important;
}

.testimonials__track {
  width: max-content !important;
  padding-left: 0 !important;
  padding-right: 30px !important;
}

.testimonials__track::before {
  content: "";
  display: block;
  flex: 0 0 clamp(180px, 22vw, 300px);
}

.testimonial-card {
  position: relative;
  z-index: 5;
}

.testimonials__title {
  z-index: 1 !important;
}

.testimonials__cards {
  position: relative;
  z-index: 4 !important;
}

@media (max-width: 1169px) {
  .testimonials::before {
    display: none;
  }
}

/* =========================================================
   RTL / LTR LANGUAGE SUPPORT – آینه‌ای کامل
   ========================================================= */
.app {
  direction: ltr;
  text-align: left;
}

.app.is-rtl {
  direction: rtl;
}

/* --- پایه: معکوس کردن جهت متن‌ها --- */
.app.is-rtl h1,
.app.is-rtl h2,
.app.is-rtl h3,
.app.is-rtl h4,
.app.is-rtl h5,
.app.is-rtl h6,
.app.is-rtl p,
.app.is-rtl span,
.app.is-rtl blockquote,
.app.is-rtl a {
  direction: rtl;
}

.app.is-rtl .header__inner,
.app.is-rtl .cta__box,
.app.is-rtl .contact-row {
  direction: rtl;
}

/* --- هدر دسکتاپ (پهنای ≥1170px) --- */
@media (min-width: 1170px) {
  .app.is-rtl .header__inner {
    flex-direction: row;
  }
  .app.is-rtl .header__inner .brand {
    order: 1;
    margin: 0;
  }
  .app.is-rtl .header__inner .nav {
    position: absolute;
    left: 32%;
    transform: translateX(-50%);
    order: 2;
    margin: 0;
  }
  .app.is-rtl .header__inner .setting-btn {
    order: 3;
    margin: 0;
  }
  .app.is-rtl .nav a {
    text-align: center;
  }
}

/* --- هدر تبلت/موبایل: جای برند، منو و دکمه زبان --- */
@media (max-width: 1168px) {
  .app.is-rtl .header__inner .brand {
    right: 28px !important;
    left: auto !important;
  }
  .app.is-rtl .menu-btn {
    left: 24px !important;
    right: auto !important;
  }
  .app.is-rtl .setting-btn {
    right: 16px !important;
    left: auto !important;
  }
  .app.is-rtl .nav {
    left: 20px !important;
    right: auto !important;
    transform-origin: top left !important;
  }
  .app.is-rtl .nav a {
    text-align: right !important;
  }
}

/* --- Hero Desktop (≥1170px) --- */
@media (min-width: 1170px) {
  .app.is-rtl .hero__image {
    right: auto !important;
    left: -360px !important;
    transform: rotate(-42deg) !important;
    border-right: 42px solid var(--blue) !important;
    border-left: 0 !important;
  }
  .app.is-rtl .hero__image img {
    transform: rotate(42deg) translate(130px, 30px) !important;
  }
  .app.is-rtl .hero__text {
    text-align: right;
  }
}

/* --- Hero Tablet/Mobile (max-width:1168px) --- */
@media (max-width: 1168px) {
  .app.is-rtl .hero__image {
    left: auto !important;
    right: 0 !important;
    clip-path: polygon(
      0 0,
      100% 0,
      100% calc(100% - 205px),
      175px 100%
    ) !important;
  }
  .app.is-rtl .hero__image img {
    left: auto !important;
    right: 0 !important;
    clip-path: polygon(
      0 0,
      100% 0,
      100% calc(100% - 180px),
      152px 100%
    ) !important;
  }
}

/* --- About: المان‌های absolute معکوس شوند --- */
.app.is-rtl .about::before {
  left: auto !important;
  right: -110px !important;
}
.app.is-rtl .about::after {
  left: auto !important;
  right: 92px !important;
}

.app.is-rtl .diamond-image {
  left: auto !important;
  right: 110px !important;
}
.app.is-rtl .soft-diamond--one {
  left: auto !important;
  right: -45px !important;
}

/* --- Expertise: مدار و آیکن‌ها معکوس شوند --- */
.app.is-rtl .expertise-orbit {
  margin-left: 0 !important;
  margin-right: 95px !important;
}
.app.is-rtl .diamond-title--large {
  left: auto !important;
  right: 106px !important;
}
.app.is-rtl .icon-bubble--camera {
  left: auto !important;
  right: -15px !important;
}
.app.is-rtl .icon-bubble--play {
  left: auto !important;
  right: 192px !important;
}
.app.is-rtl .icon-bubble--wifi {
  left: auto !important;
  right: 45px !important;
}
.app.is-rtl .icon-bubble--star {
  right: auto !important;
  left: -4px !important;
}
.app.is-rtl .icon-bubble--lab {
  right: auto !important;
  left: 55px !important;
}
.app.is-rtl .icon-bubble--small {
  left: auto !important;
  right: -48px !important;
}

/* --- Office: آفست عنوان --- */
.app.is-rtl .office-title {
  margin: 0 35px 90px 0;
}

/* --- Case Studies: sidebar و overlay --- */
.app.is-rtl .case__sidebar {
  text-align: right;
}
.app.is-rtl .case__sidebar ul {
  direction: rtl;
}
.app.is-rtl .case-card__overlay {
  text-align: right;
}

/* --- Footer --- */
.app.is-rtl .footer__brand,
.app.is-rtl .footer-col {
  text-align: right;
}

/* =========================================================
   LANGUAGE SWITCH — SMOOTH CROSSFADE
   ========================================================= */
.app {
  transition: opacity 0.18s ease;
}

.app.is-switching {
  opacity: 0.35;
}

.header,
.nav,
.hero__text,
.about__content,
.expertise__content,
.testimonial-card__text,
.case-card__overlay,
.contact-card,
.footer {
  transition: opacity 0.18s ease;
}

.app.is-switching .header,
.app.is-switching .nav,
.app.is-switching .hero__text,
.app.is-switching .about__content,
.app.is-switching .expertise__content,
.app.is-switching .footer {
  opacity: 0.5;
}

/* =========================================================
   ACCESSIBILITY — VISIBLE FOCUS STATES
   ========================================================= */
a:focus-visible,
button:focus-visible,
select:focus-visible {
  outline: 3px solid var(--blue);
  outline-offset: 3px;
  border-radius: 4px;
}

/* =========================================================
   PAGE LOAD ENTRANCE
   ========================================================= */
.header {
  animation: headerDrop 0.6s var(--ease) both;
}

@keyframes headerDrop {
  from {
    opacity: 0;
    transform: translateY(-16px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* =========================================================
   TESTIMONIALS — ENGLISH LAYOUT IS THE ONLY LAYOUT
   Persian changes text direction only; geometry never flips.
   ========================================================= */
.testimonials,
.testimonials__inner,
.testimonials__cards,
.testimonials__track,
.testimonial-card,
.testimonial-card__box,
.testimonial-card__client,
.testimonial-dots {
  direction: ltr !important;
}

.testimonials__title {
  left: 0 !important;
  right: auto !important;
}

.testimonials__track {
  padding-left: clamp(180px, 22vw, 300px) !important;
  padding-right: 30px !important;
}

.testimonials__track::before {
  display: block !important;
  content: "" !important;
  flex: 0 0 clamp(180px, 22vw, 300px) !important;
}

.testimonials__track::after {
  display: none !important;
  content: none !important;
}

.testimonial-card__box {
  border-radius: 45px 45px 45px 0 !important;
}

/* Long testimonials scroll vertically inside the fixed card. */
.testimonial-card__text {
  height: 150px !important;
  max-height: 150px !important;
  min-height: 0 !important;
  overflow-x: hidden !important;
  overflow-y: auto !important;
  padding-right: 6px;
  padding-left: 0;
  box-sizing: border-box;
  overscroll-behavior: contain;
  scrollbar-width: thin;
  direction: ltr !important;
  text-align: left !important;
  font-size: 13px !important;
}

.testimonial-card__text::-webkit-scrollbar {
  width: 3px;
}

.testimonial-card__text::-webkit-scrollbar-track {
  background: transparent;
}

.testimonial-card__text::-webkit-scrollbar-thumb {
  background: #b8eaf2;
  border-radius: 10px;
}

.testimonial-card__text::-webkit-scrollbar-thumb:hover {
  background: #7fd9ea;
}

/* Persian: only the actual text is RTL. */
.app.is-rtl .testimonial-card__text {
  direction: rtl !important;
  text-align: right !important;
  padding-right: 0;
  padding-left: 6px;
}

.app.is-rtl .testimonial-diamond h2 {
  direction: rtl !important;
  text-align: center !important;
}

.app.is-rtl .testimonial-card__client {
  direction: ltr !important;
}

.app.is-rtl .testimonial-card__client h3,
.app.is-rtl .testimonial-card__client span {
  direction: ltr !important;
  text-align: left !important;
}

@media (max-width: 1199px) {
  .testimonials__title {
    left: 0 !important;
    right: auto !important;
  }

  .testimonials__cards {
    margin-left: 100px !important;
    margin-right: 0 !important;
    width: calc(100% - 100px) !important;
  }

  .testimonials__track {
    padding-left: clamp(150px, 20vw, 250px) !important;
    padding-right: 30px !important;
  }

  .testimonial-card__text {
    height: 130px !important;
    max-height: 130px !important;
    font-size: 12px !important;
  }
}

@media (max-width: 992px) {
  .testimonials__title {
    left: auto !important;
    right: auto !important;
  }

  .testimonials__cards {
    width: calc(100% + 24px) !important;
    margin-left: -12px !important;
    margin-right: 0 !important;
    padding-left: 12px !important;
    padding-right: 12px !important;
  }

  .testimonials__track {
    padding-left: 4px !important;
    padding-right: 4px !important;
  }

  .testimonials__track::before {
    display: none !important;
    flex-basis: 0 !important;
  }

  .testimonial-card__text {
    height: 120px !important;
    max-height: 120px !important;
  }
}

@media (max-width: 576px) {
  .testimonials__title {
    left: auto !important;
    right: auto !important;
  }

  .testimonials__cards {
    width: calc(100% + 24px) !important;
    margin-left: -12px !important;
    margin-right: 0 !important;
    padding-left: 12px !important;
    padding-right: 12px !important;
  }

  .testimonials__track {
    padding-left: 4px !important;
    padding-right: 14px !important;
  }

  .testimonial-card__text {
    height: 110px !important;
    max-height: 110px !important;
    font-size: 12px !important;
  }
}

/* =========================================================
   TESTIMONIALS – فاصله ثابت ۵px از گوشه‌ها
   ========================================================= */
.testimonials {
  padding-left: 5px !important;
  padding-right: 5px !important;
}

.testimonials__inner {
  padding-left: 0 !important;
  padding-right: 0 !important;
}

/* اسلایدر: عرض را بر اساس فاصله‌ی ۵px تنظیم می‌کنیم */
.testimonials__cards {
  width: calc(100% - 10px) !important; /* 5px از چپ + 5px از راست */
  margin-left: 5px !important;
  margin-right: 5px !important;
  padding-left: 0 !important;
  padding-right: 0 !important;
}

/* عنوان چرخشی (Diamond) را ۵px از لبه چپ قرار می‌دهیم */
.testimonials__title {
  left: 5px !important;
}

/* لوپ کارت‌ها را طوری تنظیم می‌کنیم که اولین آیتم پشت عنوان نرود و از حاشیه بیرون نزند */
.testimonials__track {
  padding-left: clamp(180px, 22vw, 300px) !important;
  padding-right: 5px !important;
}

/* RTL: آینه‌سازی موقعیت‌ها برای زبان فارسی */
.app.is-rtl .testimonials__title {
  left: auto !important;
  right: 5px !important;
}

.app.is-rtl .testimonials__cards {
  margin-left: 0 !important;
  margin-right: 5px !important;
}

.app.is-rtl .testimonials__track {
  padding-left: 5px !important;
  padding-right: clamp(180px, 22vw, 300px) !important;
}

/* در تبلت و موبایل: فاصله‌ها حفظ شود */
@media (max-width: 1199px) {
  .testimonials__cards {
    width: calc(100% - 10px) !important;
    margin-left: 5px !important;
    margin-right: 5px !important;
  }

  .testimonials__track {
    padding-left: clamp(150px, 20vw, 250px) !important;
    padding-right: 5px !important;
  }

  .app.is-rtl .testimonials__track {
    padding-left: 5px !important;
    padding-right: clamp(150px, 20vw, 250px) !important;
  }
}

@media (max-width: 992px) {
  .testimonials__cards {
    width: calc(100% - 10px) !important;
    margin-left: 5px !important;
    margin-right: 5px !important;
    margin-top: 70px !important; /* ← این خط را اضافه کنید */
  }

  .testimonials__track {
    padding-left: 5px !important;
    padding-right: 5px !important;
  }

  .testimonials__track::before {
    display: none !important;
  }

  /* عنوان در موبایل وسط‌چین می‌شود، اما همچنان از گوشه فاصله دارد */
  .testimonials__title {
    left: 5px !important;
  }

  .app.is-rtl .testimonials__title {
    left: auto !important;
    right: 5px !important;
  }
}

@media (max-width: 1168px) {
  .nav:not(.active) {
    visibility: hidden !important;
    opacity: 0 !important;
    pointer-events: none !important;
    transform: translateY(-12px) scale(0.98) !important;
    transition: none !important;
    animation: none !important;
  }
  .nav.active {
    visibility: visible !important;
    opacity: 1 !important;
    pointer-events: auto !important;
    transform: translateY(0) scale(1) !important;
    transition:
      opacity 0.2s ease,
      transform 0.2s ease,
      visibility 0s linear 0s !important;
  }
  .app.is-rtl .nav {
    left: auto !important;
    right: 20px !important;
    transform-origin: top right !important;
  }
  .app.is-rtl .nav a {
    text-align: right !important;
    direction: rtl !important;
  }
  .app.is-rtl .nav-language {
    direction: rtl !important;
    text-align: right !important;
  } /* During a resize the menu is
hard-locked closed. */
  html.is-resizing .nav,
  html.is-resizing .nav.active {
    visibility: hidden !important;
    opacity: 0 !important;
    pointer-events: none !important;
    transform: translateY(-12px) scale(0.98) !important;
    transition: none !important;
    animation: none !important;
  }
}

@media (max-width: 768px) {
  .app.is-rtl .hero__image {
    left: auto !important;
    right: auto !important;
    transform: none !important;
    clip-path: none !important;
  }

  .app.is-rtl .hero__image img {
    transform: none !important;
    clip-path: none !important;
  }
}

/* ==============================
   LOADING INTRO — ADDED ONLY
   ============================== */
.ray-loader {
  position: fixed;
  inset: 0;
  z-index: 99999;
  overflow: hidden;
  background: #ffffff;
  isolation: isolate;
}

.ray-loader__pieces,
.ray-loader__pieces::before {
  position: absolute;
  inset: 0;
}

.ray-loader__pieces {
  filter: saturate(1.08) contrast(1.03);
}

.ray-loader__pieces::before {
  content: "";
  background:
    radial-gradient(
      circle at 50% 48%,
      rgba(255, 255, 255, 1) 0 8%,
      rgba(255, 255, 255, 0.9) 17%,
      transparent 38%
    ),
    radial-gradient(
      circle at 18% 24%,
      rgba(46, 91, 255, 0.13),
      transparent 28%
    ),
    radial-gradient(
      circle at 82% 22%,
      rgba(24, 76, 170, 0.16),
      transparent 30%
    ),
    radial-gradient(
      circle at 18% 78%,
      rgba(0, 167, 160, 0.14),
      transparent 31%
    ),
    radial-gradient(
      circle at 82% 78%,
      rgba(199, 154, 59, 0.13),
      transparent 30%
    ),
    linear-gradient(120deg, rgba(78, 108, 190, 0.06), transparent 38%),
    linear-gradient(300deg, rgba(16, 54, 108, 0.08), transparent 44%);
  transform: scale(0.96);
  animation: ray-loader-glow 2.55s cubic-bezier(0.16, 1, 0.3, 1) 0.08s both;
}

.ray-loader__pieces::after {
  content: "";
  position: absolute;
  inset: -22%;
  background: conic-gradient(
    from 18deg at 50% 50%,
    transparent 0deg,
    rgba(42, 102, 214, 0.09) 48deg,
    transparent 88deg,
    rgba(47, 84, 177, 0.09) 138deg,
    transparent 186deg,
    rgba(0, 156, 150, 0.09) 234deg,
    transparent 278deg,
    rgba(184, 138, 45, 0.09) 324deg,
    transparent 360deg
  );
  animation: ray-loader-ambient 3s cubic-bezier(0.16, 1, 0.3, 1) 0.02s both;
}

.ray-piece {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 42vw;
  height: 44vh;
  border: 1px solid rgba(255, 255, 255, 0.66);
  box-shadow:
    inset 0 0 56px rgba(255, 255, 255, 0.28),
    0 28px 86px rgba(20, 48, 86, 0.1);
  transform: translate(-50%, -50%) scale(1.2);
  transform-origin: center;
  will-change: transform, opacity, filter;
  animation: ray-piece-release 1.82s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

.ray-piece--1 {
  background: linear-gradient(145deg, #eef3ff 0%, #b9ccff 52%, #7299f2 100%);
  clip-path: polygon(0 0, 100% 4%, 88% 100%, 7% 88%);
  --x: -44vw;
  --y: -38vh;
  --r: -9deg;
  animation-delay: 1.42s;
}
.ray-piece--2 {
  background: linear-gradient(145deg, #f6f3df 0%, #e9d39a 55%, #c79a3b 100%);
  clip-path: polygon(6% 0, 96% 8%, 100% 96%, 0 100%);
  --x: -15vw;
  --y: -41vh;
  --r: -4deg;
  animation-delay: 1.49s;
}
.ray-piece--3 {
  background: linear-gradient(145deg, #edf2ff 0%, #b9c9f4 52%, #6d86c7 100%);
  clip-path: polygon(4% 4%, 100% 0, 94% 100%, 0 90%);
  --x: 15vw;
  --y: -40vh;
  --r: 5deg;
  animation-delay: 1.56s;
}
.ray-piece--4 {
  background: linear-gradient(145deg, #e8faf7 0%, #9edfd6 52%, #49aaa1 100%);
  clip-path: polygon(0 6%, 92% 0, 100% 100%, 8% 94%);
  --x: 43vw;
  --y: -36vh;
  --r: 10deg;
  animation-delay: 1.63s;
}
.ray-piece--5 {
  background: linear-gradient(145deg, #e8f2fb 0%, #9fc2e8 55%, #4d82bf 100%);
  clip-path: polygon(0 0, 94% 8%, 100% 100%, 6% 92%);
  --x: -48vw;
  --y: 31vh;
  --r: -11deg;
  animation-delay: 1.6s;
}
.ray-piece--6 {
  background: linear-gradient(145deg, #eef1fb 0%, #b7c1df 52%, #788db8 100%);
  clip-path: polygon(6% 0, 100% 4%, 92% 100%, 0 94%);
  --x: -16vw;
  --y: 37vh;
  --r: -5deg;
  animation-delay: 1.67s;
}
.ray-piece--7 {
  background: linear-gradient(145deg, #f8f5ec 0%, #e7d5ad 54%, #b98b42 100%);
  clip-path: polygon(0 8%, 100% 0, 94% 94%, 8% 100%);
  --x: 16vw;
  --y: 38vh;
  --r: 6deg;
  animation-delay: 1.74s;
}
.ray-piece--8 {
  background: linear-gradient(145deg, #e9f4f8 0%, #9acbd1 52%, #4e9ca4 100%);
  clip-path: polygon(7% 0, 100% 8%, 92% 100%, 0 92%);
  --x: 44vw;
  --y: 32vh;
  --r: 11deg;
  animation-delay: 1.81s;
}
.ray-piece--9 {
  background:
    radial-gradient(
      circle at 38% 34%,
      rgba(255, 255, 255, 0.96),
      transparent 34%
    ),
    linear-gradient(135deg, #eef3fb 0%, #b9c8e8 38%, #8eb7d0 70%, #7db8aa 100%);
  clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
  width: 30vw;
  height: 30vw;
  --x: 0;
  --y: 0;
  --r: 45deg;
  animation-delay: 0.08s;
  box-shadow:
    0 0 120px rgba(47, 84, 177, 0.16),
    0 0 70px rgba(42, 102, 214, 0.11),
    inset 0 0 44px rgba(255, 255, 255, 0.34);
  z-index: 2;
}

.ray-loader__center {
  position: absolute;
  inset: 0;
  z-index: 4;
  display: grid;
  place-content: center;
  justify-items: center;
  text-align: center;
  pointer-events: none;
  animation: ray-center-exit 0.82s cubic-bezier(0.16, 1, 0.3, 1) 1.74s forwards;
}

.ray-loader__mark {
  width: 86px;
  height: 86px;
  display: grid;
  place-items: center;
  background: linear-gradient(
    145deg,
    #173b67 0%,
    #285d9a 42%,
    #2b8f9f 72%,
    #6aaea0 100%
  );
  border: 1px solid rgba(255, 255, 255, 0.7);
  border-radius: 21px;
  transform: rotate(45deg) scale(0.68);
  box-shadow:
    0 22px 64px rgba(23, 59, 103, 0.22),
    0 0 0 11px rgba(255, 255, 255, 0.46),
    0 0 0 24px rgba(44, 91, 153, 0.07),
    0 0 46px rgba(0, 156, 150, 0.1);
  animation: ray-mark-in 1.05s cubic-bezier(0.16, 1, 0.3, 1) 0.18s both;
}

.ray-loader__mark::before {
  content: "";
  position: absolute;
  width: 48%;
  height: 48%;
  border-radius: 14px;
  border: 1px solid rgba(255, 255, 255, 0.32);
  box-shadow: inset 0 0 14px rgba(255, 255, 255, 0.18);
}

.ray-loader__mark::after {
  content: "RP";
  color: #fff;
  font-size: 18px;
  font-weight: 900;
  letter-spacing: -0.02em;
  transform: rotate(-45deg);
  text-shadow: 0 2px 14px rgba(18, 45, 79, 0.16);
}

.ray-loader__mark span {
  display: none;
}

.ray-loader__brand {
  margin-top: 29px;
  color: #173b67;
  font-size: clamp(22px, 2.4vw, 36px);
  font-weight: 800;
  letter-spacing: 0.12em;
  text-shadow: 0 8px 30px rgba(23, 59, 103, 0.12);
  animation: ray-brand-in 0.94s cubic-bezier(0.16, 1, 0.3, 1) 0.36s both;
}

.ray-loader__brand::after {
  content: "";
  display: block;
  width: 58px;
  height: 2px;
  margin: 14px auto 0;
  border-radius: 999px;
  background: linear-gradient(
    90deg,
    transparent,
    #2b6bd6,
    #4d73b6,
    #3a9eb0,
    #72a99d,
    transparent
  );
  opacity: 0.76;
}

.ray-loader__flash {
  position: absolute;
  inset: -15%;
  z-index: 5;
  background: radial-gradient(
    circle at center,
    rgba(255, 255, 255, 0.98) 0 6%,
    rgba(235, 242, 248, 0.86) 13%,
    rgba(213, 232, 239, 0.42) 24%,
    transparent 46%
  );
  opacity: 0;
  pointer-events: none;
  mix-blend-mode: screen;
  animation: ray-flash 1.12s cubic-bezier(0.16, 1, 0.3, 1) 2.58s forwards;
}

.ray-loader-active,
.ray-loader-active body {
  overflow: hidden !important;
}

.ray-loader-leave-active {
  transition: opacity 0.42s cubic-bezier(0.16, 1, 0.3, 1);
}
.ray-loader-leave-to {
  opacity: 0;
}
.ray-loader-enter-active {
  transition: opacity 0.16s ease;
}
.ray-loader-enter-from {
  opacity: 0;
}

@keyframes ray-mark-in {
  0% {
    opacity: 0;
    transform: rotate(45deg) scale(0.34);
    filter: blur(9px);
  }
  52% {
    opacity: 1;
    transform: rotate(45deg) scale(1.06);
    filter: blur(0);
  }
  76% {
    transform: rotate(45deg) scale(0.97);
  }
  100% {
    opacity: 1;
    transform: rotate(45deg) scale(0.94);
    filter: blur(0);
  }
}

@keyframes ray-brand-in {
  0% {
    opacity: 0;
    transform: translateY(18px);
    filter: blur(10px);
    letter-spacing: 0.21em;
  }
  58% {
    opacity: 1;
    filter: blur(0);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
    filter: blur(0);
    letter-spacing: 0.12em;
  }
}

@keyframes ray-center-exit {
  0% {
    opacity: 1;
    transform: scale(1);
    filter: blur(0);
  }
  58% {
    opacity: 1;
    transform: scale(1.018);
    filter: blur(0);
  }
  100% {
    opacity: 0;
    transform: scale(0.965);
    filter: blur(4px);
  }
}

@keyframes ray-piece-release {
  0% {
    opacity: 1;
    filter: blur(0);
    transform: translate(-50%, -50%) scale(1.2) rotate(0);
  }
  30% {
    opacity: 1;
    filter: blur(0);
    transform: translate(-50%, -50%) scale(1.15) rotate(calc(var(--r) * 0.08));
  }
  58% {
    opacity: 1;
    filter: blur(0.15px);
    transform: translate(
        calc(-50% + var(--x) * 0.52),
        calc(-50% + var(--y) * 0.52)
      )
      scale(1.09) rotate(calc(var(--r) * 0.48));
  }
  82% {
    opacity: 0.96;
    filter: blur(0.45px);
    transform: translate(
        calc(-50% + var(--x) * 0.88),
        calc(-50% + var(--y) * 0.88)
      )
      scale(1.035) rotate(calc(var(--r) * 0.88));
  }
  100% {
    opacity: 0;
    filter: blur(2.2px);
    transform: translate(calc(-50% + var(--x)), calc(-50% + var(--y)))
      scale(0.97) rotate(var(--r));
  }
}

@keyframes ray-loader-glow {
  0% {
    opacity: 0.1;
    transform: scale(0.92);
  }
  32% {
    opacity: 0.68;
    transform: scale(1.01);
  }
  62% {
    opacity: 0.38;
    transform: scale(1.026);
  }
  100% {
    opacity: 0.16;
    transform: scale(1);
  }
}

@keyframes ray-loader-ambient {
  0% {
    opacity: 0;
    transform: scale(0.91) rotate(-12deg);
  }
  34% {
    opacity: 0.72;
  }
  100% {
    opacity: 0.18;
    transform: scale(1.05) rotate(8deg);
  }
}

@keyframes ray-flash {
  0% {
    opacity: 0;
    transform: scale(0.7);
  }
  28% {
    opacity: 0.72;
    transform: scale(0.98);
  }
  56% {
    opacity: 0.22;
    transform: scale(1.1);
  }
  100% {
    opacity: 0;
    transform: scale(1.3);
  }
}

@media (max-width: 700px) {
  .ray-piece {
    width: 62vw;
    height: 34vh;
  }
  .ray-piece--9 {
    width: 48vw;
    height: 48vw;
  }
  .ray-loader__mark {
    width: 68px;
    height: 68px;
    border-radius: 17px;
  }
  .ray-loader__brand {
    font-size: 21px;
    letter-spacing: 0.08em;
  }
  .ray-loader__brand::after {
    width: 44px;
    margin-top: 11px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ray-loader *,
  .ray-loader {
    animation: none !important;
    transition: none !important;
  }
}

/* =========================================================
   MOBILE HEADER / NAV DIRECTION FIX
   English: brand + logo LEFT, menu RIGHT
   Persian: brand + logo RIGHT, menu LEFT
   No other UI changes.
========================================================= */
@media (max-width: 769px) {
  /* Keep the logo/text relationship stable in both languages. */
  .header__inner .brand {
    flex-direction: row !important;
  }

  /* English */
  .app:not(.is-rtl) .header__inner .brand {
    left: 15px !important;
    right: auto !important;
  }

  .app:not(.is-rtl) .menu-btn {
    left: auto !important;
    right: 15px !important;
    align-items: flex-end !important;
  }

  .app:not(.is-rtl) .nav {
    left: auto !important;
    right: 14px !important;
    transform-origin: top right !important;
  }

  /* Persian */
  .app.is-rtl .header__inner .brand {
    left: auto !important;
    right: 15px !important;
  }

  .app.is-rtl .menu-btn {
    left: 15px !important;
    right: auto !important;
    align-items: flex-start !important;
  }

  .app.is-rtl .nav {
    left: 14px !important;
    right: auto !important;
    transform-origin: top left !important;
  }

  .app.is-rtl .nav a,
  .app.is-rtl .header.scrolled .nav a {
    text-align: right !important;
  }
}

/* =========================================================
   FOOTER — FINAL RESPONSIVE FIX ≤ 420px
   ========================================================= */
@media (max-width: 420px) {
  .footer {
    width: 100%;
    overflow: hidden;
    padding: 48px 0 30px;
  }

  .footer__inner {
    width: 100%;
    max-width: 100%;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 30px;
    padding: 0 18px;
    margin: 0;
  }

  /* Brand */
  .footer__brand {
    width: 100%;
    max-width: 100%;
    min-width: 0;
    text-align: center;
  }

  .brand--footer {
    display: inline-flex;
    justify-content: center;
    max-width: 100%;
  }

  .brand--footer .brand__text {
    max-width: calc(100vw - 100px);
    font-size: 18px !important;
    white-space: normal !important;
    overflow-wrap: anywhere;
    word-break: break-word;
  }

  .brand--footer .brand__mark {
    flex: 0 0 34px;
  }

  .footer__brand p {
    width: 100%;
    max-width: 100%;
    margin: 16px auto 12px;
    font-size: 12px;
    line-height: 1.8;
    overflow-wrap: anywhere;
    word-break: break-word;
  }

  .footer__brand small {
    display: block;
    width: 100%;
    font-size: 11px;
    overflow-wrap: anywhere;
  }

  /* Footer columns */
  .footer-col {
    width: 100%;
    max-width: 100%;
    min-width: 0;
    text-align: center !important;
  }

  .footer-col h4 {
    margin-bottom: 16px;
    font-size: 12px;
  }

  .footer-col a {
    width: 100%;
    max-width: 100%;
    margin-bottom: 11px;
    padding-inline: 8px;
    font-size: 12px;
    line-height: 1.6;
    overflow-wrap: anywhere;
    word-break: break-word;
  }

  /* Social */
  .footer-social {
    width: 100%;
    max-width: 100%;
    min-width: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .socials {
    justify-content: center;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 20px;
  }

  .socials a {
    width: 32px;
    height: 32px;
    flex: 0 0 32px;
  }

  .footer-social select {
    width: 100%;
    max-width: 240px;
    min-width: 0;
    height: 40px;
    padding: 0 12px;
    font-size: 12px;
  }

  /* RTL */
  .app.is-rtl .footer__brand,
  .app.is-rtl .footer-col,
  .app.is-rtl .footer-social {
    text-align: center !important;
  }

  .app.is-rtl .footer-col a {
    transform: none !important;
  }
}

/* =========================================================
    FOOTER ACCORDION — MOBILE ≤ 420px
    ========================================================= */

.footer-col__toggle {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 0;
  margin: 0;
  border: 0;
  background: transparent;
  color: var(--blue-dark);
  font: inherit;
  font-size: 12px;
  font-weight: 900;
  cursor: pointer;
  text-align: left;
}

.footer-col__arrow {
  display: none;
  font-size: 18px;
  line-height: 1;
  transition: transform 0.3s var(--ease);
}

.footer-col__links {
  display: flex;
  flex-direction: column;
}

/* =========================
   MOBILE
   ========================= */

@media (max-width: 420px) {
  .footer {
    width: 100%;
    overflow: hidden;
    padding: 48px 0 30px;
  }

  .footer__inner {
    width: 100%;
    max-width: 100%;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    padding: 0 18px;
    margin: 0;
  }

  /* Brand */
  .footer__brand {
    width: 100%;
    max-width: 100%;
    min-width: 0;
    text-align: center;
    margin-bottom: 28px;
  }

  .brand--footer {
    display: inline-flex;
    justify-content: center;
  }

  .brand--footer .brand__text {
    font-size: 18px !important;
    white-space: normal !important;
    overflow-wrap: anywhere;
  }

  .footer__brand p {
    width: 100%;
    max-width: 100%;
    margin: 16px auto 12px;
    font-size: 12px;
    line-height: 1.8;
    overflow-wrap: anywhere;
    word-break: break-word;
  }

  .footer__brand small {
    display: block;
    font-size: 11px;
  }

  /* =========================
     Accordion sections
     ========================= */

  .footer-col {
    width: 100%;
    max-width: 100%;
    min-width: 0;
    margin: 0;
    border-top: 1px solid #e8eef3;
    text-align: inherit !important;
  }

  .footer-col:last-of-type {
    border-bottom: 1px solid #e8eef3;
  }

  .footer-col__toggle {
    min-height: 56px;
    padding: 0 4px;
    font-size: 13px;
  }

  .footer-col__arrow {
    display: block;
  }

  .footer-col.is-open .footer-col__arrow {
    transform: rotate(180deg);
  }

  .footer-col__links {
    max-height: 0;
    opacity: 0;
    overflow: hidden;
    pointer-events: none;
    padding: 0 4px;
    transition:
      max-height 0.4s var(--ease),
      opacity 0.25s ease,
      padding 0.35s var(--ease);
  }

  .footer-col.is-open .footer-col__links {
    max-height: 300px;
    opacity: 1;
    pointer-events: auto;
    padding-bottom: 14px;
  }

  .footer-col__links a {
    display: block;
    width: 100%;
    max-width: 100%;
    margin: 0;
    padding: 9px 0;
    font-size: 12px;
    line-height: 1.6;
    color: #8c96a4;
    overflow-wrap: anywhere;
    word-break: break-word;
    transform: none !important;
  }

  .footer-col__links a:hover {
    color: var(--blue-dark);
    transform: none !important;
  }

  /* Social */
  .footer-social {
    width: 100%;
    max-width: 100%;
    min-width: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 28px;
  }

  .socials {
    justify-content: center;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 20px;
  }

  .footer-social select {
    width: 100%;
    max-width: 240px;
    min-width: 0;
    height: 40px;
    font-size: 12px;
  }

  /* =========================
     RTL
     ========================= */

  .app.is-rtl .footer-col__toggle {
    direction: rtl;
    text-align: right;
  }

  .app.is-rtl .footer-col__links {
    text-align: right;
  }

  .app.is-rtl .footer-col__links a {
    transform: none !important;
  }
}

/* =========================================================
   SERVICES SLIDER — KEEP GEOMETRY LTR
   RTL فقط روی متن اعمال می‌شود، نه روی مکانیزم اسلایدر
   ========================================================= */

.services__slider,
.services__track {
  direction: ltr !important;
}

/* در فارسی فقط محتوای کارت RTL باشد */
.app.is-rtl .service-card__body {
  direction: rtl;
  text-align: right;
}

.app.is-rtl .service-card__btn {
  direction: rtl;
}

/* فلش دکمه همچنان ظاهر درست خودش را حفظ کند */
.app.is-rtl .service-card__arrow {
  direction: ltr;
}
</style>
