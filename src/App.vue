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
    <!-- =========================================================
         LOADER
         ========================================================= -->
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

    <!-- =========================================================
         HEADER
         ========================================================= -->
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

    <!-- =========================================================
         HERO
         ========================================================= -->
    <section id="home" class="hero section">
      <div class="hero__image">
        <img :src="getImage('pic1.jpg')" alt="Hero" />
      </div>

      <div class="container hero__content">
        <div class="hero__text reveal reveal--visible">
          <h1>{{ t("Create") }} {{ t("Business Solution") }}</h1>

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

    <!-- =========================================================
         ABOUT
         ========================================================= -->
    <section id="about" class="about section">
      <div class="container about__inner">
        <div class="about__visual reveal reveal--left" v-reveal>
          <!-- Keep the original large blue diamond -->
          <div class="about-blue-diamond" aria-hidden="true"></div>

          <!-- Small image diamond: upper-left -->
          <div
            class="about-image-diamond about-image-diamond--one"
            aria-hidden="true"
          >
            <img :src="getImage('pic2.jpg')" alt="" draggable="false" />
          </div>

          <!-- Small image diamond: lower-left -->
          <div
            class="about-image-diamond about-image-diamond--two"
            aria-hidden="true"
          >
            <img :src="getImage('pic2.jpg')" alt="" draggable="false" />
          </div>

          <div class="about-card">
            <div class="diamond-title">
              <span>
                {{ tl("about")[0] }}<br />
                {{ tl("about")[1] }}
              </span>
            </div>

            <div class="about-image diamond-image">
              <img :src="getImage('pic2.jpg')" alt="Office" />
            </div>
          </div>
        </div>

        <div class="about__responsive-title">
          <span class="about__responsive-title-bar"></span>
          <span>{{ currentLanguage === "fa" ? "درباره ما" : "About Us" }}</span>
        </div>

        <div class="about__content reveal reveal--right" v-reveal>
          <h2>
            {{ t("Nulla lobortis nunc vitae nisi semper semper velit") }}
          </h2>

          <div class="stats">
            <div class="stat">
              <strong class="stat__value">
                <span class="stat__plus">+</span>
                <span class="stat__number">{{ animatedStats.employee }}</span>
              </strong>

              <span class="stat__line" aria-hidden="true"></span>

              <span>{{ t("Employee") }}</span>
            </div>

            <div class="stat">
              <strong class="stat__value">
                <span class="stat__plus">+</span>
                <span class="stat__number">{{ animatedStats.projects }}</span>
              </strong>

              <span class="stat__line" aria-hidden="true"></span>

              <span>{{ t("Projects") }}</span>
            </div>

            <div class="stat">
              <strong class="stat__value">
                <span class="stat__plus">+</span>
                <span class="stat__number">{{ animatedStats.clients }}</span>
              </strong>

              <span class="stat__line" aria-hidden="true"></span>

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

    <!-- =========================================================
         SERVICES
         ========================================================= -->
    <section id="services" class="services section">
      <div class="services__floating-title">
        <div class="container">
          <div class="services__title-wrapper">
            <div class="services__title-icon">
              <span class="services__icon">◆</span>
              <h2>
                {{ tl("services")[0] }}<br />
                {{ tl("services")[1] }}
              </h2>
            </div>
          </div>
        </div>
      </div>

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
    </section>

    <!-- =========================================================
         EXPERTISE
         ========================================================= -->
    <section id="expertise" class="expertise section">
      <div class="container expertise__inner">
        <div class="expertise__visual reveal reveal--left" v-reveal>
          <div class="expertise-orbit">
            <div class="orbit-line orbit-line--one"></div>
            <div class="orbit-line orbit-line--two"></div>

            <div class="expertise-title diamond-title diamond-title--large">
              <span>
                {{ tl("expertise")[0] }}<br />
                {{ tl("expertise")[1] }}
              </span>
            </div>

            <div class="icon-bubble icon-bubble--camera" aria-hidden="true">
              <svg viewBox="0 0 24 24">
                <rect x="3" y="6.5" width="18" height="13" rx="3"></rect>
                <path d="M8 6.5l1.4-2h5.2l1.4 2"></path>
                <circle cx="12" cy="13" r="3.2"></circle>
              </svg>
            </div>

            <div class="icon-bubble icon-bubble--play" aria-hidden="true">
              <svg viewBox="0 0 24 24">
                <path d="M8 5.5v13l10-6.5L8 5.5Z"></path>
              </svg>
            </div>

            <div class="icon-bubble icon-bubble--wifi" aria-hidden="true">
              <svg viewBox="0 0 24 24">
                <path d="M3.5 8.5a13.5 13.5 0 0 1 17 0"></path>
                <path d="M6.5 12a8.5 8.5 0 0 1 11 0"></path>
                <path d="M9.5 15.5a4 4 0 0 1 5 0"></path>
                <circle cx="12" cy="19" r="1"></circle>
              </svg>
            </div>

            <div class="icon-bubble icon-bubble--star" aria-hidden="true">
              <svg viewBox="0 0 24 24">
                <path
                  d="m12 3 2.1 5.4 5.9.4-4.5 3.8 1.4 5.7L12 15.1l-4.9 3.2 1.4-5.7L4 8.8l5.9-.4L12 3Z"
                ></path>
              </svg>
            </div>

            <div class="icon-bubble icon-bubble--lab" aria-hidden="true">
              <svg viewBox="0 0 24 24">
                <path d="M9 3h6"></path>
                <path
                  d="M10 3v5l-5 9.2A2.8 2.8 0 0 0 7.5 21h9a2.8 2.8 0 0 0 2.5-3.8L14 8V3"
                ></path>
                <path d="M8 15h8"></path>
              </svg>
            </div>

            <div class="icon-bubble icon-bubble--small" aria-hidden="true">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="3"></circle>
                <path d="M12 2v4"></path>
                <path d="M12 18v4"></path>
                <path d="m4.9 4.9 2.8 2.8"></path>
                <path d="m16.3 16.3 2.8 2.8"></path>
                <path d="M2 12h4"></path>
                <path d="M18 12h4"></path>
                <path d="m4.9 19.1 2.8-2.8"></path>
                <path d="m16.3 7.7 2.8-2.8"></path>
              </svg>
            </div>
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

    <!-- =========================================================
         TESTIMONIALS
         ========================================================= -->
    <section id="testimonials" class="testimonials section">
      <div class="container testimonials__inner">
        <div class="testimonials__title reveal reveal--left" v-reveal>
          <div class="testimonial-diamond">
            <div class="testimonial-quote">“</div>

            <h2>
              {{ tl("testimonials")[0] }}<br />
              {{ tl("testimonials")[1] }}
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

    <!-- =========================================================
         CASE STUDIES
         ========================================================= -->
    <section id="case-studies" class="case section">
      <div class="container case__inner">
        <aside class="case__sidebar reveal reveal--left" v-reveal>
          <h2>
            {{ tl("case")[0] }}<br v-if="tl('case')[1]" />
            {{ tl("case")[1] }}
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

            <div class="case-card__overlay">
              <span class="case-icon">◆</span>

              <h3>{{ t(project.title) }}</h3>

              <p>{{ t("Vestibulum consequat hendrerit.") }}</p>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- =========================================================
         CTA
         ========================================================= -->
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

    <!-- =========================================================
         OFFICE / CONTACT
         ========================================================= -->
    <section id="contact" class="office section">
      <div class="container office__inner">
        <div class="office__left reveal reveal--left" v-reveal>
          <div class="diamond-title office-title">
            <span>
              {{ tl("office")[0] }}<br />
              {{ tl("office")[1] }}
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
          <img :src="getImage('pic19.jpg')" alt="Map" />

          <div class="map-pin"></div>

          <button class="map-btn" type="button">
            {{ t("Services ◆") }}
          </button>
        </div>
      </div>
    </section>

    <!-- =========================================================
         FOOTER
         ========================================================= -->
    <footer class="footer">
      <div class="container footer__inner">
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

        <div class="footer-social">
          <div class="socials">
            <a href="#" aria-label="Facebook">f</a>
            <a href="#" aria-label="LinkedIn">in</a>
            <a href="#" aria-label="X">x</a>
            <a href="#" aria-label="Instagram">ig</a>
          </div>

          <select
            v-model="currentLanguage"
            @change="changeLanguage(currentLanguage)"
            aria-label="Language"
          >
            <option value="en">{{ t("English - En") }}</option>
            <option value="fa">{{ t("Persian - Fa") }}</option>
          </select>
        </div>
      </div>
    </footer>

    <!-- =========================================================
         BACK TO TOP
         ========================================================= -->
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

/* =========================================================
   IMAGE HELPERS
   ========================================================= */
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
   SEO
   ========================================================= */
const SEO = {
  en: {
    title: "Raybod Pouye | Intelligent Software & Digital Solutions",
    description:
      "Raybod Pouye provides intelligent software, AI, knowledge management, network management, data analytics, enterprise integration, and network security solutions for organizations.",
    locale: "en_US",
  },
  fa: {
    title: "رایبد پویه | راهکارهای هوشمند نرم‌افزاری و سازمانی",
    description:
      "رایبد پویه در زمینه تولید نرم‌افزارهای هوشمند، هوش مصنوعی، مدیریت دانش، مدیریت شبکه، تحلیل داده، یکپارچه‌سازی سازمانی و امنیت شبکه فعالیت می‌کند.",
    locale: "fa_IR",
  },
};

const setMeta = (name, content, attribute = "name") => {
  if (!content) return;

  let element = document.head.querySelector(`meta[${attribute}="${name}"]`);

  if (!element) {
    element = document.createElement("meta");
    element.setAttribute(attribute, name);
    document.head.appendChild(element);
  }

  element.setAttribute("content", content);
};

const setLink = (rel, href, id = "") => {
  let element = id
    ? document.head.querySelector(`#${id}`)
    : document.head.querySelector(`link[rel="${rel}"]`);

  if (!element) {
    element = document.createElement("link");
    element.rel = rel;
    if (id) element.id = id;
    document.head.appendChild(element);
  }

  element.href = href;
};

const updateSEO = () => {
  if (typeof document === "undefined") return;

  const content = SEO[currentLanguage.value] ?? SEO.en;
  const canonicalUrl = `${window.location.origin}${window.location.pathname}`;

  document.documentElement.lang = currentLanguage.value;
  document.documentElement.dir = currentLanguage.value === "fa" ? "rtl" : "ltr";

  document.title = content.title;

  setMeta("description", content.description);

  setMeta(
    "robots",
    "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1",
  );

  setMeta("author", "Raybod Pouye");
  setMeta("theme-color", "#ffffff");

  setMeta("og:type", "website", "property");
  setMeta("og:title", content.title, "property");
  setMeta("og:description", content.description, "property");
  setMeta("og:url", canonicalUrl, "property");
  setMeta("og:site_name", "Raybod Pouye", "property");
  setMeta("og:locale", content.locale, "property");

  setMeta("twitter:card", "summary", "name");
  setMeta("twitter:title", content.title, "name");
  setMeta("twitter:description", content.description, "name");

  setLink("canonical", canonicalUrl, "seo-canonical");

  let schema = document.head.querySelector("#raybod-pouye-schema");

  if (!schema) {
    schema = document.createElement("script");
    schema.id = "raybod-pouye-schema";
    schema.type = "application/ld+json";
    document.head.appendChild(schema);
  }

  schema.textContent = JSON.stringify({
    "@context": "https://schema.org",
    "@graph": [
      {
        "@type": "Organization",
        "@id": `${canonicalUrl}#organization`,
        name: currentLanguage.value === "fa" ? "رایبد پویه" : "Raybod Pouye",
        url: canonicalUrl,
        email: "info@raybidpouye.com",
        description: content.description,
      },
      {
        "@type": "WebSite",
        "@id": `${canonicalUrl}#website`,
        url: canonicalUrl,
        name: content.title,
        description: content.description,
        inLanguage: currentLanguage.value,
        publisher: { "@id": `${canonicalUrl}#organization` },
      },
      {
        "@type": "ItemList",
        name:
          currentLanguage.value === "fa"
            ? "خدمات رایبد پویه"
            : "Raybod Pouye Services",
        itemListElement: servicesList.map((service, index) => ({
          "@type": "ListItem",
          position: index + 1,
          name: t(service.title),
          description: t(service.desc),
        })),
      },
    ],
  });
};

/* =========================================================
   FOOTER ACCORDION
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
   SERVICES DATA
   ========================================================= */
const servicesList = [
  {
    title: "service1",
    desc: "service1_desc",
    img: getImage("pic3.jpg"),
  },
  {
    title: "service2",
    desc: "service2_desc",
    img: getImage("pic4.jpg"),
  },
  {
    title: "service3",
    desc: "service3_desc",
    img: getImage("pic5.jpg"),
  },
  {
    title: "service4",
    desc: "service4_desc",
    img: getImage("pic6.jpg"),
  },
  {
    title: "service5",
    desc: "service5_desc",
    img: getImage("pic7.jpg"),
  },
  {
    title: "service6",
    desc: "service6_desc",
    img: getImage("pic8.jpg"),
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
    image: getImage("pic9.jpg"),
  },
  {
    text: "test2_text",
    name: "مهندس رضا صادقی",
    role: "role2",
    rating: 5,
    image: getImage("pic10.jpg"),
  },
  {
    text: "test3_text",
    name: "دکتر آرین مطاعی",
    role: "role3",
    rating: 5,
    image: getImage("pic11.jpg"),
  },
  {
    text: "test4_text",
    name: "X",
    role: "role4",
    rating: 5,
    image: getImage("pic12.jpg"),
  },
  {
    text: "test5_text",
    name: "Y",
    role: "role5",
    rating: 5,
    image: getImage("pic13.jpg"),
  },
  {
    text: "test6_text",
    name: "Z",
    role: "role6",
    rating: 5,
    image: getImage("pic14.jpg"),
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

    Employee: "Experience",
    Projects: "Projects",
    Clients: "Organizations",

    "Aliquam lobortis magna neque, gravida consequat velit venenatis at.":
      "Leveraging advanced technologies and a dedicated expert team, we provide intelligent services and products that help organizations improve their performance.",

    Our: "Our",

    service1: "Intelligent Network Management Software",
    service1_desc:
      "A comprehensive software for intelligent management and monitoring of organizational networks with automated troubleshooting capabilities.",

    service2: "Intelligent Search Engine & GPT Assistant",
    service2_desc:
      "An intelligent search engine with a GPT assistant that enables quick access to information across various organizational sources.",

    service3: "Raya Intelligent Knowledge Network",
    service3_desc:
      "Raya intelligent knowledge network, a platform for managing and exchanging knowledge within the organization.",

    service4: "Data Analytics Platform",
    service4_desc:
      "An advanced data analytics platform for organizational data with interactive and precise reporting.",

    service5: "Enterprise Integration Platform",
    service5_desc:
      "An integrated enterprise platform for connecting various organizational systems and facilitating data flow.",

    service6: "Network Security Solution",
    service6_desc:
      "A comprehensive network security solution with threat detection and rapid response to attacks.",

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
   SECTION TITLES
   ========================================================= */
const titles = {
  en: {
    about: ["About", "Us"],
    services: ["Our", "Services"],
    expertise: ["Our", "Expertise"],
    testimonials: ["Client", "Testimonials"],
    case: ["Case", "Studies"],
    office: ["Our", "Office"],
  },

  fa: {
    about: ["درباره", "ما"],
    services: ["خدمات", "ما"],
    expertise: ["تخصص", "ما"],
    testimonials: ["نظرات", "مشتریان"],
    case: ["پروژه‌ها", ""],
    office: ["دفتر", "ما"],
  },
};

const tl = (key) => titles[currentLanguage.value]?.[key] ?? titles.en[key];

/* =========================================================
   LANGUAGE SWITCH
   ========================================================= */
const changeLanguage = (language) => {
  if (isSwitchingLanguage.value || language === currentLanguage.value) {
    return;
  }

  isSwitchingLanguage.value = true;

  window.setTimeout(() => {
    currentLanguage.value = language;
    updateSEO();

    nextTick(() => {
      checkStatsVisibility();
      alignTestimonialSlider();
    });

    window.setTimeout(() => {
      isSwitchingLanguage.value = false;
    }, 30);
  }, 180);
};

const toggleLanguage = () => {
  changeLanguage(currentLanguage.value === "en" ? "fa" : "en");
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
    image: getImage("pic15.jpg"),
  },
  {
    title: "proj2",
    class: "case-card--medium",
    image: getImage("pic18.jpg"),
  },
  {
    title: "proj3",
    class: "case-card--small",
    image: getImage("pic16.jpg"),
  },
  {
    title: "proj4",
    class: "case-card--wide",
    image: getImage("pic17.jpg"),
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
   ALIGN TESTIMONIAL SLIDER
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
  updateSEO();

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

  window.addEventListener("scroll", handleScroll, {
    passive: true,
  });

  window.addEventListener("resize", handleResize, {
    passive: true,
  });

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
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=Vazirmatn:wght@400;500;600;700;800;900&display=swap");

/* ============ 1. TOKENS / RESET ============ */
:root {
  --blue: #269ff3;
  --blue-dark: #0751af;
  --cyan: #9eeff3;
  --text: #242833;
  --muted: #8b96a6;
  --shadow: 0 18px 45px rgba(18, 77, 126, 0.13);
  --shadow-soft: 0 10px 30px rgba(17, 80, 140, 0.12);
  --container: 1180px;
  --ease: cubic-bezier(0.16, 1, 0.3, 1);
}

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
  background: #fff;
  overflow-x: hidden;
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

a:focus-visible,
button:focus-visible,
select:focus-visible {
  outline: 3px solid var(--blue);
  outline-offset: 3px;
  border-radius: 4px;
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

.app {
  --dir: 1;
  --rot: 45deg;
  width: 100%;
  overflow: hidden;
  text-align: start;
  transition: opacity 0.18s ease;
}

.app.is-rtl {
  --dir: -1;
  --rot: -45deg;
  font-family: "Vazirmatn", "Inter", sans-serif;
}

.app.is-switching {
  opacity: 0.35;
}

.container {
  width: min(var(--container), calc(100% - 52px));
  margin-inline: auto;
}

.section {
  position: relative;
}

/* ============ 2. REVEAL ============ */
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

/* ============ 3. KEYFRAMES ============ */
@keyframes headerDrop {
  from {
    opacity: 0;
    transform: translateY(-16px);
  }

  to {
    opacity: 1;
    transform: none;
  }
}

@keyframes fadeDown {
  from {
    opacity: 0;
    transform: translateY(-25px);
  }

  to {
    opacity: 1;
    transform: none;
  }
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-60px);
  }

  to {
    opacity: 1;
    transform: none;
  }
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }

  to {
    opacity: 1;
    transform: none;
  }
}

/* ============ 4. BUTTON ============ */
.btn {
  position: relative;
  overflow: hidden;
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
}

.btn--primary {
  color: #03a3c4;
  background: #fff;
  border: 1px solid #d2edf6;
  box-shadow: 0 14px 28px rgba(28, 137, 199, 0.13);
}

.btn--primary span {
  display: inline-block;
  transition: transform 0.3s var(--ease);
  transform: scaleX(var(--dir));
}

.btn--primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 18px 34px rgba(28, 137, 199, 0.18);
  border-color: var(--blue);
}

.btn--primary:hover span {
  transform: scaleX(var(--dir)) translateX(4px);
}

.btn--primary:active {
  transform: translateY(-1px) scale(0.98);
}

/* ============ 5. HEADER ============ */
.header {
  position: fixed;
  inset: 0 0 auto 0;
  z-index: 100;
  height: 58px;
  transition:
    background 0.3s ease,
    box-shadow 0.3s ease,
    backdrop-filter 0.3s ease;
  animation: headerDrop 0.6s var(--ease) both;
}

.header.scrolled {
  background: #fff;
  box-shadow: 0 8px 26px rgba(20, 95, 160, 0.1);
  backdrop-filter: blur(16px);
}

.header__inner {
  width: min(965px, calc(100% - 52px));
  height: 100%;
  display: flex;
  align-items: center;
  gap: 32px;
}

.brand {
  display: flex;
  align-items: center;
  gap: 9px;
  flex-shrink: 0;
  font-weight: 800;
  color: #2a2c32;
  transition: transform 0.25s var(--ease);
}

.brand:hover {
  transform: translateY(-1px);
}

.brand__mark {
  position: relative;
  overflow: hidden;
  isolation: isolate;
  display: grid;
  place-items: center;
  width: 36px;
  height: 36px;
  border-radius: 9px;
  background: linear-gradient(135deg, #37cae0, #0e55a7, #37cae0);
  transform: rotate(45deg);
  transition: transform 0.35s var(--ease);
}

.brand:hover .brand__mark {
  transform: rotate(45deg) scale(1.08);
}

.brand__mark::after {
  content: "RP";
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  font-weight: 900;
  color: #fff;
  transform: rotate(-45deg);
}

.brand__text {
  font-size: 17px;
  font-weight: 700;
  color: inherit;
}

.nav {
  display: flex;
  align-items: center;
  gap: 43px;
  margin-inline-start: auto;
}

.nav a {
  position: relative;
  font-size: 15px;
  font-weight: 800;
  line-height: 1.25;
  color: #fff;
  transition: color 0.25s;
}

.nav a::after {
  content: "";
  position: absolute;
  inset-inline: 0;
  bottom: -4px;
  height: 2px;
  border-radius: 4px;
  background: var(--cyan);
  transform: scaleX(0);
  transition: transform 0.3s var(--ease);
}

.nav a:hover,
.nav a.is-active {
  color: var(--cyan);
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

.header.scrolled .nav a:hover,
.header.scrolled .nav a.is-active {
  color: var(--blue-dark);
}

.nav-language,
.menu-btn {
  display: none;
}

.lang-btn {
  flex-shrink: 0;
  width: 34px;
  height: 34px;
  display: grid;
  place-items: center;
  overflow: hidden;
  border: 0;
  border-radius: 50%;
  background: #fff;
  color: var(--blue-dark);
  font-size: 13px;
  font-weight: 800;
  cursor: pointer;
  box-shadow: 0 8px 22px rgba(23, 81, 138, 0.13);
  transition:
    transform 0.3s var(--ease),
    box-shadow 0.3s var(--ease);
}

.lang-btn:hover {
  transform: translateY(-2px) rotate(8deg);
  box-shadow: 0 12px 26px rgba(23, 81, 138, 0.22);
}

@media (min-width: 1169px) {
  .header__inner .brand {
    position: fixed;
    top: 11px;
    inset-inline-start: 120px;
    z-index: 110;
  }
}

@media (max-width: 1168px) {
  .about::before {
    display: none;
  }
}

/* ============ 6. HERO ============ */
.hero {
  padding-top: 0;
}

.hero__image {
  position: absolute;
  z-index: 0;
  overflow: hidden;
}

.hero__image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero__content {
  position: relative;
  z-index: 2;
  display: flex;
}

.hero__scroll-cue {
  display: none;
}

.hero h1 {
  margin: 0 0 24px;
  font-weight: 900;
}

.hero p {
  margin: 0 0 34px;
}

@media (min-width: 1169px) {
  .hero {
    isolation: isolate;
    height: 450px;
    background: #fff;
  }

  .hero__image {
    top: -430px;
    inset-inline-start: calc(65.3% - 350px);
    width: 820px;
    height: 820px;
    border: 27px solid var(--blue);
    border-radius: 55px;
    background: #000;
    transform: rotate(var(--rot));
  }

  .hero__image img {
    position: absolute;
    inset: 0;
    filter: brightness(0.4);
    transform: rotate(calc(var(--rot) * -1)) scale(1.41421356);
  }

  .hero__image::before {
    content: "";
    position: absolute;
    inset: 0;
    z-index: 2;
    background: rgba(0, 0, 0, 0.05);
    pointer-events: none;
  }

  .hero::before,
  .hero::after,
  .hero__content::before,
  .hero__content::after {
    content: "";
    position: absolute;
    z-index: -1;
    pointer-events: none;
    transform: rotate(var(--rot));
  }

  .hero::before {
    inset-inline-start: calc(65.3% - 175.5px);
    top: 140px;
    width: 240px;
    height: 290px;
    border-radius: 48px;
    background: rgba(210, 238, 252, 0.95);
  }

  .hero::after {
    inset-inline-start: calc(65.3vw + 47.5px);
    top: 350px;
    width: 140px;
    height: 140px;
    border-radius: 40px;
    background: rgba(196, 232, 250, 0.97);
  }

  .hero__content::before {
    inset-inline-start: 55px;
    top: 185px;
    width: 190px;
    height: 190px;
    border-radius: 38px;
    background: rgba(207, 237, 252, 0.94);
  }

  .hero__content::after {
    inset-inline-start: 205px;
    top: 225px;
    width: 145px;
    height: 145px;
    border-radius: 26px;
    background: rgba(225, 243, 255, 0.92);
  }

  .hero__content {
    height: 450px;
    align-items: flex-start;
  }

  .hero__text {
    width: 430px;
    margin-top: 142px;
  }

  .hero h1 {
    font-size: 33px;
    line-height: 1.18;
    letter-spacing: -1.35px;
    color: #26282c;
  }

  .hero p {
    width: 370px;
    max-width: 370px;
    font-size: 19px;
    line-height: 1.47;
    font-weight: 600;
    color: #2c2d30;
  }

  .hero .btn--primary {
    width: 154px;
    min-width: 154px;
    height: 38px;
    min-height: 38px;
    padding: 0 18px;
    gap: 12px;
    border: 1px solid rgba(18, 191, 229, 0.75);
    border-radius: 3px;
    background: rgba(255, 255, 255, 0.12);
    backdrop-filter: blur(12px) saturate(140%);
    color: #03aeca;
    font-size: 11px;
    letter-spacing: 0.15px;
    box-shadow:
      0 4px 12px rgba(20, 150, 200, 0.08),
      inset 0 1px 0 rgba(255, 255, 255, 0.25);
  }

  .hero .btn--primary span {
    width: 0;
    height: 0;
    margin-inline-start: 2px;
    border-block: 6px solid transparent;
    border-left: 10px solid #05b8dc;
    font-size: 0;
    line-height: 0;
  }
}

/* ============ 7. ABOUT ============ */
.about {
  padding: 150px 0 120px;
}

.about::before {
  content: "";
  position: absolute;
  inset-inline-start: -110px;
  top: 195px;
  width: 310px;
  height: 310px;
  border-radius: 48px;
  background: rgba(228, 246, 255, 0.95);
  transform: rotate(45deg);
  margin-inline-start: 200px;
}
.about__inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  min-height: 420px;
}

.about__visual {
  position: relative;
  height: 420px;
}

.about-card {
  position: absolute;
  inset-inline-start: 75px;
  top: 65px;
  width: 380px;
  height: 280px;
}

.diamond-title {
  position: relative;
  z-index: 2;
  display: grid;
  place-items: center;
  width: 196px;
  height: 196px;
  border-radius: 28px;
  background: linear-gradient(135deg, #17b3fb, #2389ef);
  color: #fff;
  transform: rotate(45deg);
  box-shadow: 0 16px 32px rgba(28, 144, 232, 0.23);
  transition:
    transform 0.4s var(--ease),
    box-shadow 0.4s var(--ease);
}

.diamond-title span {
  font-size: 36px;
  line-height: 1.15;
  font-weight: 900;
  letter-spacing: -1px;
  transform: rotate(-45deg);
}

.app.is-rtl .diamond-title span,
.app.is-rtl .services__title-icon h2 {
  text-align: center;
}

.about-card:hover .diamond-title {
  transform: rotate(45deg) scale(1.04);
  box-shadow: 0 20px 40px rgba(28, 144, 232, 0.32);
}

.diamond-image {
  position: absolute;
  z-index: 1;
  top: -28px;
  inset-inline-start: 140px;
  width: 205px;
  height: 205px;
  overflow: hidden;
  border-radius: 32px;
  box-shadow: var(--shadow);
  transform: rotate(45deg);
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
}

/* Preserve the original large pale-blue diamond */
.about-blue-diamond {
  position: absolute;
  z-index: 0;
  inset-inline-start: -45px;
  top: 105px;
  width: 250px;
  height: 250px;
  border-radius: 42px;
  background: rgba(224, 246, 255, 0.86);
  transform: rotate(45deg);
  pointer-events: none;
}

/* Two matching small image diamonds */
.about-image-diamond {
  position: absolute;
  width: 92px;
  height: 92px;
  overflow: hidden;
  border-radius: 18px;
  background: #fff;
  box-shadow: 0 12px 28px rgba(17, 80, 140, 0.18);
  transform: rotate(45deg);
  pointer-events: none;
}

.about-image-diamond img {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 145%;
  height: 145%;
  max-width: none;
  object-fit: cover;
  transform: translate(-50%, -50%) rotate(-45deg) scale(1.02);
}

.about-image-diamond--one {
  inset-inline-start: 184px;
  top: -36px;
}

.about-image-diamond--two {
  inset-inline-start: 207px;
  top: 248px;
}

.about__content {
  max-width: 530px;
  padding-top: 30px;
}

.about__responsive-title {
  display: none;
}

.about__responsive-title-bar {
  display: inline-block;
  width: 34px;
  height: 5px;
  flex: 0 0 34px;
  border-radius: 999px;
  background: var(--blue);
}

.about__content h2,
.expertise__content h2 {
  margin: 0 0 35px;
  font-size: 25px;
  line-height: 1.45;
  letter-spacing: -0.7px;
  font-weight: 900;
}

.stats {
  display: flex;
  gap: 62px;
  margin-bottom: 44px;
}

.stat {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.stat__value {
  display: inline-flex;
  align-items: baseline;
  flex-direction: row;
  direction: ltr;
  gap: 2px;
  margin: 0;
  font-size: 32px;
  font-weight: 900;
  color: #111b2c;
  font-variant-numeric: tabular-nums;
  line-height: 1;
}

.stat__plus {
  order: 1;
  font-size: 20px;
  font-weight: 900;
  color: var(--blue);
}

.stat__number {
  order: 2;
  display: inline-block;
}

.stat__line {
  display: block;
  width: 55px;
  height: 5px;
  margin-top: 7px;
  margin-bottom: 8px;
  border-radius: 20px;
  background: var(--blue);
}

.stat > span:last-child {
  color: #9aa3af;
  font-size: 16px;
}

blockquote {
  margin: 0;
  padding-inline-start: 28px;
  border-inline-start: 4px solid #d9d9d9;
  color: #b6bbc3;
  font-size: 22px;
  line-height: 1.45;
  font-weight: 700;
  font-style: italic;
}

.app.is-rtl blockquote {
  font-style: normal;
}

/* ============ 8. SERVICES ============ */
.services {
  position: relative;
  overflow: hidden;
  background: #eef9ff;
  padding: 80px 0 90px;
}

.services::before {
  content: "";
  position: absolute;
  top: -130px;
  inset-inline-end: 60px;
  z-index: 0;
  width: 240px;
  height: 240px;
  border-radius: 42px;
  background: rgba(255, 255, 255, 0.95);
  transform: rotate(45deg);
  pointer-events: none;
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
  font-size: 32px;
  line-height: 1;
  color: var(--blue);
}

.services__title-icon h2 {
  margin: 0;
  font-size: clamp(32px, 4vw, 48px);
  line-height: 1.15;
  font-weight: 900;
  color: var(--blue-dark);
}

.services__slider,
.services__track,
.services__arrows {
  direction: ltr;
}

.services__arrows {
  position: relative;
  z-index: 5;
  top: 15px;
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  width: min(var(--container), calc(100% - 52px));
  margin-inline: auto;
}

.app.is-rtl .services__arrows {
  justify-content: flex-start;
}

.services__arrow-btn {
  width: 48px;
  height: 48px;
  display: grid;
  place-items: center;
  border: 0;
  border-radius: 50%;
  background: #fff;
  color: var(--blue-dark);
  font-size: 28px;
  line-height: 1;
  cursor: pointer;
  box-shadow: var(--shadow-soft);
  transition: 0.25s var(--ease);
}

.services__arrow-btn:hover {
  background: var(--blue);
  color: #fff;
  transform: translateY(-2px);
  box-shadow: 0 12px 24px rgba(38, 159, 243, 0.25);
}

.services__slider {
  position: relative;
  z-index: 2;
  overflow-x: auto;
  padding: 20px 0 30px;
  scrollbar-width: none;
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
  padding-inline: calc((100vw - var(--container)) / 2);
}

.services__spacer {
  flex: 0 0 40px;
  min-width: 40px;
}

.service-card {
  flex: 0 0 280px;
  width: 280px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border-radius: 14px;
  background: #fff;
  box-shadow: var(--shadow-soft);
  transition:
    transform 0.35s var(--ease),
    box-shadow 0.35s var(--ease);
}

.app.is-rtl .service-card {
  direction: rtl;
}

.service-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 38px rgba(17, 80, 140, 0.18);
}

.service-card__media {
  flex-shrink: 0;
  height: 160px;
  overflow: hidden;
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
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 22px 24px 26px;
}

.service-card__body h3 {
  margin: 0 0 10px;
  font-size: 17px;
  font-weight: 800;
}

.service-card__body p {
  flex: 1;
  margin: 0 0 12px;
  font-size: 13px;
  line-height: 1.6;
  color: var(--muted);
}

.service-card__btn {
  align-self: flex-start;
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
}

.service-card__btn:hover {
  background: var(--blue);
  color: #fff;
}

.service-card__arrow {
  display: inline-block;
  transform: scaleX(var(--dir));
  transition: transform 0.25s var(--ease);
}

.service-card__btn:hover .service-card__arrow {
  transform: scaleX(var(--dir)) translateX(4px);
}

/* ============ 9. EXPERTISE ============ */
.expertise {
  padding: 120px 0 160px;
}

.expertise::after {
  content: "";
  position: absolute;
  bottom: -140px;
  inset-inline-start: -90px;
  width: 250px;
  height: 250px;
  border-radius: 55px;
  background: #edf8fd;
  transform: rotate(45deg);
}

.expertise__inner {
  display: grid;
  grid-template-columns: 1.05fr 1fr;
  gap: 80px;
  align-items: center;
}

.expertise__visual {
  position: relative;
  min-height: 430px;
}

.expertise__responsive-title {
  display: none;
}

.expertise-orbit {
  position: relative;
  width: 430px;
  height: 430px;
  margin-inline-start: 95px;
}

.orbit-line {
  position: absolute;
  inset: 45px;
  border: 3px solid rgba(38, 48, 243, 0.12);
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
  to {
    transform: rotate(405deg);
  }
}

.diamond-title--large {
  position: absolute;
  top: 103px;
  inset-inline-start: 106px;
  width: 225px;
  height: 225px;
  background: linear-gradient(135deg, #1fb2fb, #2089ec);
}

.diamond-title--large::before {
  content: "";
  position: absolute;
  inset: -55px;
  z-index: -1;
  border-radius: 45px;
  background: rgba(38, 159, 243, 0.15);
}

.icon-bubble {
  position: absolute;
  display: grid;
  place-items: center;
  border-radius: 50%;
  color: #fff;
  font-weight: 900;
  box-shadow: var(--shadow-soft);
  animation: iconFloat 5s ease-in-out infinite;
}

.icon-bubble svg {
  width: 42%;
  height: 42%;
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
  pointer-events: none;
}

.icon-bubble--small svg {
  width: 52%;
  height: 52%;
}

@keyframes iconFloat {
  50% {
    transform: translateY(-10px);
  }
}

.icon-bubble--camera {
  width: 70px;
  height: 70px;
  background: var(--blue);
  inset-inline-start: -15px;
  top: 94px;
}

.icon-bubble--play {
  width: 70px;
  height: 70px;
  background: #12c3ce;
  top: 30px;
  inset-inline-start: 192px;
  animation-delay: 0.6s;
}

.icon-bubble--wifi {
  width: 54px;
  height: 54px;
  background: #02bfb8;
  inset-inline-start: 45px;
  bottom: 92px;
  animation-delay: 1.2s;
}

.icon-bubble--star {
  width: 36px;
  height: 36px;
  background: var(--blue);
  inset-inline-end: -4px;
  top: 190px;
  animation-delay: 1.8s;
}

.icon-bubble--lab {
  width: 80px;
  height: 80px;
  background: #681be7;
  inset-inline-end: 55px;
  bottom: 35px;
  animation-delay: 2.4s;
}

.icon-bubble--small {
  width: 34px;
  height: 34px;
  background: #8647ff;
  inset-inline-start: -48px;
  bottom: 50px;
  animation-delay: 3s;
}

.expertise__content {
  position: relative;
  z-index: 2;
  max-width: 610px;
}

.expertise__content p {
  margin: 0 0 28px;
  color: #485363;
  line-height: 1.9;
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
  border-radius: 3px;
  color: #9aa3af;
  text-align: center;
  font-size: 12px;
  font-weight: 600;
  transition:
    transform 0.25s var(--ease),
    box-shadow 0.25s var(--ease),
    border-color 0.25s var(--ease),
    color 0.25s var(--ease);
}

.tags span:hover {
  transform: translateY(-2px);
  border-color: var(--blue);
  color: var(--blue-dark);
  box-shadow: 0 8px 18px rgba(38, 159, 243, 0.14);
}

.tags span.active {
  background: var(--blue);
  border-color: var(--blue);
  color: #fff;
}

.tags span.active:hover {
  box-shadow: 0 10px 22px rgba(38, 159, 243, 0.3);
}

/* ============ 10. TESTIMONIALS ============ */
.testimonials {
  position: relative;
  overflow: hidden;
  background: #edf8fd;
  padding: 70px 0 60px;
}

.testimonials,
.testimonials__inner,
.testimonials__cards,
.testimonials__track,
.testimonial-card,
.testimonial-dots {
  direction: ltr;
}

.testimonials::before {
  content: "";
  position: absolute;
  top: -310px;
  right: -30px;
  left: auto;
  z-index: 0;
  width: 280px;
  height: 350px;
  border-radius: 40px;
  background: rgba(255, 255, 255, 0.95);
  transform: rotate(45deg);
}

.app.is-rtl .testimonials::before {
  right: auto;
  left: -30px;
}

.app.is-ltr .testimonials::before {
  left: auto;
  right: -30px;
}

.testimonials__inner {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 100%;
  min-height: 380px;
  margin: 0;
}

.testimonials__title {
  position: absolute;
  top: 50%;
  left: 0;
  z-index: 1;
  width: 240px;
  height: 240px;
  transform: translateY(-50%);
  pointer-events: none;
}

.testimonial-diamond {
  position: relative;
  top: -10%;
  width: 230px;
  height: 100%;
  display: grid;
  place-items: center;
  transform: rotate(45deg);
}

.testimonial-diamond h2 {
  position: relative;
  z-index: 2;
  margin: 0;
  font:
    700 28px/1.15 Georgia,
    "Times New Roman",
    serif;
  color: #222;
  white-space: nowrap;
  text-align: center;
  transform: rotate(-45deg);
}

.app.is-rtl .testimonial-diamond h2 {
  font-family: "Vazirmatn", serif;
  direction: rtl;
}

.testimonial-quote {
  position: absolute;
  top: 55px;
  left: 80px;
  color: #ff5a3d;
  font:
    70px/1 Georgia,
    serif;
  transform: rotate(-45deg);
}

.testimonials__cards {
  position: relative;
  z-index: 4;
  width: 100%;
  margin: 0;
  padding: 20px 0 35px;
  overflow-x: auto;
  scrollbar-width: none;
  cursor: grab;
  touch-action: pan-y;
  overscroll-behavior-x: contain;
  -webkit-overflow-scrolling: touch;
  user-select: none;
  -webkit-user-select: none;
}

.testimonials__cards::-webkit-scrollbar {
  display: none;
}

.testimonials__cards.is-dragging {
  cursor: grabbing;
  scroll-behavior: auto;
}

.testimonials__track {
  display: flex;
  align-items: stretch;
  gap: 20px;
  width: max-content;
  padding: 0 0 10px;
}

.testimonials__track::before {
  content: "";
  flex: 0 0 clamp(180px, 22vw, 300px);
}

.testimonial-card {
  position: relative;
  z-index: 5;
  flex: 0 0 clamp(230px, 24vw, 300px);
  min-width: 0;
}

.testimonial-card img {
  pointer-events: none;
  -webkit-user-drag: none;
}

.testimonial-card__box {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 230px;
  padding: 22px 24px;
  border-radius: 45px 45px 45px 0;
  background: #fff;
  box-shadow: 0 10px 18px rgba(40, 86, 110, 0.08);
  transition:
    transform 0.35s var(--ease),
    box-shadow 0.35s var(--ease);
}

.testimonial-card:hover .testimonial-card__box {
  transform: translateY(-4px);
  box-shadow: 0 16px 28px rgba(40, 86, 110, 0.12);
}

.testimonial-card__text {
  height: 150px;
  margin: 0;
  padding-right: 6px;
  overflow-y: auto;
  overscroll-behavior: contain;
  scrollbar-width: thin;
  color: #272b30;
  font-size: 13px;
  line-height: 1.55;
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

.app.is-rtl .testimonial-card__text {
  direction: rtl;
  padding-right: 0;
  padding-left: 6px;
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
  flex: 0 0 56px;
  width: 56px;
  height: 56px;
  object-fit: cover;
  border: 3px solid #fff;
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(35, 68, 88, 0.15);
}

.testimonial-card__client h3 {
  margin: 0 0 3px;
  font-size: 17px;
  font-weight: 700;
  color: #272b30;
}

.testimonial-card__client span {
  font-size: 12px;
  font-style: italic;
  color: #535a60;
}

.app.is-rtl .testimonial-card__info {
  direction: rtl;
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

/* ============ 11. CASE STUDIES ============ */
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
  margin: 32px 0 48px;
  font-size: 42px;
  line-height: 1.05;
  font-weight: 900;
  color: var(--blue-dark);
}

.case__sidebar ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.case__sidebar li {
  margin-bottom: 13px;
  padding: 11px 18px;
  border-radius: 5px;
  color: #263345;
  font-size: 14px;
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
  background: #fff;
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

.case-card__overlay {
  position: absolute;
  inset: auto 0 0 0;
  padding: 18px;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.65), transparent);
  color: #fff;
}

.case-card__overlay h3 {
  margin: 0;
  font-size: 16px;
  line-height: 1.28;
}

.case-card__overlay p {
  margin: 5px 0 0;
  font-size: 11px;
  opacity: 0.9;
}

.case-icon {
  display: grid;
  place-items: center;
  width: 31px;
  height: 31px;
  margin-bottom: 8px;
  border-radius: 50%;
  background: #fff;
  color: var(--blue);
  transition: transform 0.35s var(--ease);
}

.case-card:hover .case-icon {
  transform: scale(1.15) rotate(15deg);
}

/* ============ 12. CTA ============ */
.cta {
  background: #eef9ff;
  padding: 32px 0 150px;
}

.cta__box {
  display: flex;
  justify-content: space-between;
  align-items: center;
  min-height: 138px;
  padding: 30px 56px;
  border: 2px solid #15bed5;
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 15px 35px rgba(30, 150, 210, 0.08);
  transition:
    box-shadow 0.35s var(--ease),
    transform 0.35s var(--ease);
}

.cta__box:hover {
  transform: translateY(-3px);
  box-shadow: 0 22px 46px rgba(30, 150, 210, 0.16);
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

/* ============ 13. OFFICE ============ */
.office {
  background: #eef9ff;
  padding: 45px 0 120px;
}

.office::before {
  content: "";
  position: absolute;
  top: -10px;
  inset-inline-start: 230px;
  width: 480px;
  height: 480px;
  border-radius: 62px;
  background: rgba(255, 255, 255, 0.75);
  backdrop-filter: blur(8px);
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
  margin: 0 0 90px;
  margin-inline-start: 35px;
}

.contact-card {
  max-width: 470px;
  margin-bottom: 24px;
  padding: 22px 26px;
  border-radius: 10px;
  background: #fff;
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
  flex-wrap: wrap;
  gap: 34px;
  margin-bottom: 13px;
  color: #435163;
  font-size: 12px;
}

.contact-card p {
  margin: 0;
  color: #435163;
  font-size: 12px;
}

.map {
  position: relative;
  height: 500px;
  overflow: hidden;
  border-radius: 14px;
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
  z-index: 2;
  width: 42px;
  height: 42px;
  border-radius: 50% 50% 50% 0;
  background: #ff654b;
  box-shadow: 0 8px 18px rgba(255, 101, 75, 0.3);
  transform: rotate(-45deg);
  animation: pinBounce 2.4s ease-in-out infinite;
}

.map-pin::after {
  content: "";
  position: absolute;
  inset: 12px;
  border-radius: 50%;
  background: #fff;
}

@keyframes pinBounce {
  50% {
    transform: rotate(-45deg) translateY(-8px);
  }
}

.map-btn {
  position: absolute;
  z-index: 3;
  bottom: 16px;
  inset-inline-end: 16px;
  padding: 11px 18px;
  border: 0;
  border-radius: 6px;
  background: var(--blue-dark);
  color: #fff;
  font-size: 12px;
  font-weight: 800;
  cursor: pointer;
  transition:
    transform 0.3s var(--ease),
    background 0.3s var(--ease);
}

.map-btn:hover {
  transform: translateY(-2px);
  background: var(--blue);
}

/* ============ 14. FOOTER ============ */
.footer {
  background: #fff;
  padding: 64px 0 42px;
}

.footer__inner {
  display: grid;
  grid-template-columns:
    1.5fr
    1fr
    1fr
    1fr
    1.1fr;
  gap: 48px;
}

.brand--footer .brand__mark {
  width: 34px;
  height: 34px;
  border-radius: 9px;
  background: #3153bc;
}

.brand--footer .brand__mark::after {
  font-size: 13px;
}

.brand--footer .brand__text {
  font-size: 19px;
  color: var(--blue-dark);
}

.footer__brand p {
  max-width: 310px;
  color: #a1a9b4;
  font-size: 13px;
  line-height: 1.7;
}

.footer__brand small {
  color: #a1a9b4;
  font-size: 12px;
}

.footer-col__toggle {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0;
  padding: 0;
  border: 0;
  background: transparent;
  color: var(--blue-dark);
  font: inherit;
  font-size: 12px;
  font-weight: 900;
  text-align: start;
  cursor: pointer;
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
  margin-top: 16px;
}

.footer-col a {
  display: block;
  margin-bottom: 14px;
  color: #8c96a4;
  font-size: 13px;
  transition:
    color 0.25s var(--ease),
    transform 0.25s var(--ease);
}

.footer-col a:hover {
  color: var(--blue-dark);
  transform: translateX(calc(3px * var(--dir)));
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
  border-radius: 50%;
  background: #edf7ff;
  color: var(--blue-dark);
  font-size: 12px;
  font-weight: 900;
  transition:
    transform 0.3s var(--ease),
    background 0.3s var(--ease),
    color 0.3s var(--ease);
}

.socials a:hover {
  background: var(--blue);
  color: #fff;
  transform: translateY(-3px);
}

.footer-social select {
  width: 210px;
  height: 42px;
  padding: 0 14px;
  border: 1px solid #d7e4ef;
  border-radius: 8px;
  background: #fff;
  color: var(--blue-dark);
  cursor: pointer;
  transition: border-color 0.25s ease;
}

.footer-social select:hover {
  border-color: var(--blue);
}

/* ============ 15. BACK TO TOP ============ */
.to-top {
  position: fixed;
  z-index: 99;
  bottom: 28px;
  inset-inline-end: 30px;
  width: 62px;
  height: 62px;
  border: 0;
  border-radius: 50%;
  background: #9ee7ff;
  color: var(--blue-dark);
  font-size: 34px;
  cursor: pointer;
  opacity: 0;
  visibility: hidden;
  transform: translateY(20px);
  box-shadow: 0 12px 28px rgba(22, 130, 220, 0.2);
  transition: 0.3s var(--ease);
}

.to-top.show {
  opacity: 1;
  visibility: visible;
  transform: none;
}

.to-top:hover {
  background: var(--blue);
  color: #fff;
  transform: translateY(-4px);
}

/* =========================================================
   16. ≤1200
   ========================================================= */
@media (max-width: 1200px) {
  .case__grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* =========================================================
   17. ≤1199
   ========================================================= */
@media (max-width: 1199px) {
  .testimonials {
    padding: 55px 0 50px;
  }

  .testimonials__title {
    width: 190px;
    height: 190px;
  }

  .testimonial-quote {
    left: 25px;
  }

  .testimonial-card__box {
    height: 210px;
    padding: 20px 22px;
  }

  .testimonial-card__text {
    height: 130px;
    font-size: 12px;
  }
}

/* =========================================================
   18. ≤1168
   ========================================================= */
@media (max-width: 1168px) {
  .lang-btn {
    display: none;
  }

  .testimonials::before {
    display: none;
  }

  .header {
    height: 76px;
    background: transparent;
    box-shadow: none;
    backdrop-filter: none;
  }

  .header.scrolled {
    background: rgba(255, 255, 255, 0.95);
    box-shadow: 0 10px 30px rgba(20, 95, 160, 0.1);
    backdrop-filter: blur(16px);
  }

  .header__inner .brand {
    position: fixed;
    top: 22px;
    inset-inline-start: 28px;
    z-index: 135;
    gap: 10px;
    height: 42px;
    color: #fff;
    white-space: nowrap;
    animation: fadeDown 0.7s var(--ease) both;
  }

  .header .brand__mark {
    flex: 0 0 40px;
    width: 40px;
    height: 40px;
  }

  .header .brand__text {
    font-size: 27px;
    font-weight: 800;
    line-height: 1;
    color: #fff;
    text-shadow: 0 2px 7px rgba(0, 0, 0, 0.28);
  }

  .header.scrolled .brand__text {
    color: #2a2c32;
    text-shadow: none;
  }

  .menu-btn {
    position: fixed;
    top: 20px;
    inset-inline-end: 24px;
    z-index: 140;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    justify-content: center;
    gap: 6px;
    width: 45px;
    height: 45px;
    padding: 0;
    border: 0;
    background: transparent;
    cursor: pointer;
    animation: fadeDown 0.7s 0.1s var(--ease) both;
  }

  .menu-btn span {
    display: block;
    width: 30px;
    height: 3px;
    border-radius: 20px;
    background: #111;
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
    inset-inline-end: 20px;
    z-index: 135;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    width: min(330px, calc(100vw - 40px));
    margin: 0;
    padding: 12px 16px;
    border-radius: 17px;
    background: rgba(255, 255, 255, 0.98);
    box-shadow: 0 14px 38px rgba(11, 26, 40, 0.2);
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    transform: translateY(-12px) scale(0.98);
    transform-origin: calc(50% + 50% * var(--dir)) top;
  }

  .nav.active {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
    transform: none;
    transition:
      opacity 0.2s ease,
      transform 0.2s ease;
  }

  html.is-resizing .nav,
  html.is-resizing .nav.active {
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    transition: none;
  }

  .nav a,
  .header.scrolled .nav a {
    display: block;
    padding: 14px 10px;
    border-bottom: 1px solid #edf1f4;
    color: #1f3447;
    font-size: 14px;
    font-weight: 700;
  }

  .nav a:last-of-type {
    border-bottom: 0;
  }

  .nav a:hover,
  .nav a.is-active,
  .header.scrolled .nav a:hover,
  .header.scrolled .nav a.is-active {
    color: var(--blue-dark);
  }

  .nav a::after {
    display: none;
  }

  .nav-language {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
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

  .nav-language:hover {
    color: var(--blue-dark);
    background: rgba(38, 159, 243, 0.05);
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

  .hero {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: clamp(600px, 92vh, 760px);
    min-height: clamp(600px, 92svh, 760px);
    overflow: hidden;
    background: #fff;
  }

  .hero__image {
    --clip: polygon(0 0, 100% 0, calc(100% - 175px) 100%, 0 calc(100% - 205px));

    --clip-img: polygon(
      0 0,
      100% 0,
      calc(100% - 152px) 100%,
      0 calc(100% - 180px)
    );

    top: 0;
    inset-inline-start: 0;
    width: calc(100% - 48px);
    height: 100%;
    background: var(--blue);
    clip-path: var(--clip);
    animation: slideDown 0.9s var(--ease) both;
  }

  .app.is-rtl .hero__image {
    --clip: polygon(0 0, 100% 0, 100% calc(100% - 205px), 175px 100%);

    --clip-img: polygon(0 0, 100% 0, 100% calc(100% - 180px), 152px 100%);
  }

  .hero__image img {
    position: absolute;
    top: 0;
    inset-inline-start: 0;
    width: calc(100% - 31px);
    height: calc(100% - 25px);
    filter: brightness(0.52) saturate(0.8);
    clip-path: var(--clip-img);
  }

  .hero__content {
    align-items: center;
    justify-content: center;
    width: min(650px, calc(100% - 145px));
    padding: 110px 0 70px;
    text-align: center;
    animation: fadeUp 0.8s 0.2s var(--ease) both;
  }

  .hero__text {
    width: 100%;
    margin: 18px 0 0;
  }

  .hero h1 {
    margin-bottom: 42px;
    font-size: clamp(38px, 5.1vw, 52px);
    line-height: 1.12;
    letter-spacing: -1.6px;
    color: #fff;
    text-shadow: 0 2px 7px rgba(0, 0, 0, 0.28);
  }

  .hero p {
    max-width: 480px;
    margin: 0 auto 34px;
    font-size: clamp(20px, 2.8vw, 29px);
    font-weight: 700;
    line-height: 1.48;
    color: #fff;
    text-shadow: 0 2px 6px rgba(0, 0, 0, 0.25);
  }

  .hero .btn--primary {
    min-width: 250px;
    min-height: 60px;
    padding: 0 28px;
    border: 1px solid rgba(255, 255, 255, 0.72);
    border-radius: 6px;
    background: rgba(20, 31, 41, 0.38);
    color: #fff;
    font-size: 15px;
    box-shadow: none;
  }

  .hero .btn--primary:hover {
    background: rgba(20, 31, 41, 0.55);
    box-shadow: none;
  }

  .about {
    padding: 30px 0 120px;
  }

  .expertise {
    overflow: hidden;
    padding: 58px 0 95px;
    background: #fff;
  }

  .expertise::after {
    inset: 155px auto auto 50%;
    z-index: 0;
    width: 650px;
    height: 650px;
    border-radius: 58px;
    background: #f0f8ff;
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
    font:
      700 clamp(30px, 5vw, 48px)/1 Georgia,
      "Times New Roman",
      serif;
    text-align: center;
  }

  .app.is-rtl .expertise__responsive-title {
    font-family: "Vazirmatn", serif;
  }

  .expertise__content {
    width: 100%;
    max-width: 1050px;
    text-align: center;
  }

  .expertise__content h2 {
    max-width: 850px;
    margin: 0 auto 68px;
    color: #252525;
    font-size: clamp(25px, 3.2vw, 34px);
    line-height: 1.4;
  }

  .expertise__content p {
    max-width: 1030px;
    margin: 0 auto 42px;
    color: #292d32;
    font-size: clamp(16px, 2vw, 20px);
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
    border: 1px solid #cfd8e0;
    border-radius: 6px;
    background: rgba(255, 255, 255, 0.35);
    color: #2d3339;
    font-size: 14px;
  }

  .tags span.active {
    background: #2196ed;
    border-color: #2196ed;
    color: #fff;
  }
}

/* =========================================================
   19. ≤992
   ========================================================= */
@media (max-width: 992px) {
  .container {
    width: min(calc(100% - 34px), var(--container));
  }

  .about__inner,
  .office__inner {
    grid-template-columns: 1fr;
  }

  .about__visual {
    display: none;
  }

  .about__responsive-title {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    width: 100%;
    margin: 0 auto 20px;
    text-align: center;
    color: var(--blue-dark);
    font-size: clamp(30px, 5vw, 44px);
    line-height: 1.15;
    font-weight: 900;
  }

  .about__responsive-title-bar {
    width: 34px;
    height: 5px;
    flex: 0 0 34px;
    border-radius: 999px;
    background: var(--blue);
  }

  .office-title {
    margin: 0 auto 40px;
  }

  .contact-card {
    max-width: none;
  }

  .map {
    height: 380px;
  }

  .about__content {
    margin-inline: auto;
    text-align: center;
  }

  .stats {
    justify-content: center;
  }

  blockquote {
    text-align: start;
  }

  .services__track {
    padding-inline: 20px;
  }

  .services__spacer {
    flex-basis: 20px;
    min-width: 20px;
  }

  .service-card {
    flex-basis: 260px;
    width: 260px;
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

  .testimonials {
    padding: 40px 0;
  }

  .testimonials__inner {
    display: flex;
    flex-direction: column;
    min-height: auto;
  }

  .testimonials__title {
    top: -20px;
    width: 200px;
    height: 200px;
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
    left: 32px;
    font-size: 48px;
  }

  .testimonials__cards {
    margin-top: 70px;
    padding: 0;
  }

  .testimonials__track {
    gap: 16px;
  }

  .testimonials__track::before {
    display: none;
  }

  .testimonial-card {
    flex-basis: min(68vw, 300px);
  }

  .testimonial-card__box {
    height: 200px;
  }

  .testimonial-card__text {
    height: 120px;
  }
}

/* =========================================================
   20. ≤768
   ========================================================= */
@media (max-width: 768px) {
  .header {
    height: 70px;
  }

  .header__inner .brand {
    top: 14px;
    inset-inline-start: 15px;
    height: 42px;
  }

  .header .brand__mark {
    flex-basis: 42px;
    width: 42px;
    height: 42px;
    border-radius: 11px;
  }

  .header .brand__mark::after {
    font-size: 16px;
  }

  .header .brand__text {
    font-size: 26px;
    text-shadow: 0 2px 6px rgba(0, 0, 0, 0.28);
  }

  .header.scrolled .brand__text {
    text-shadow: none;
  }

  .menu-btn {
    top: 14px;
    inset-inline-end: 15px;
    width: 42px;
    height: 42px;
    padding: 0 4px;
  }

  .menu-btn span {
    width: 29px;
    background: #fff;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.25);
  }

  .header.scrolled .menu-btn span {
    background: #111;
    box-shadow: none;
  }

  .nav {
    top: 70px;
    inset-inline-end: 14px;
    width: min(310px, calc(100vw - 28px));
  }

  .hero {
    min-height: clamp(560px, 92vh, 760px);
    min-height: clamp(560px, 92svh, 760px);
    background: #1a3542;
  }

  .hero__image {
    inset: 0;
    width: 100%;
    height: 100%;
    background: #1a3542;
    clip-path: none;
  }

  .hero__image::before {
    content: "";
    position: absolute;
    inset: 0;
    z-index: 1;
    background: rgba(7, 29, 39, 0.43);
  }

  .hero__image img {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    filter: none;
    clip-path: none;
  }

  .hero__content {
    width: min(calc(100% - 42px), 520px);
    padding: 120px 0 60px;
    animation-delay: 0.24s;
  }

  .hero__text {
    margin: 0;
  }

  .hero h1 {
    margin-bottom: 38px;
    font-size: clamp(29px, 7.4vw, 39px);
    line-height: 1.15;
    letter-spacing: -1px;
    text-shadow: 0 2px 7px rgba(0, 0, 0, 0.3);
  }

  .hero p {
    max-width: 450px;
    font-size: clamp(18px, 4.8vw, 24px);
    line-height: 1.48;
    text-shadow: 0 2px 6px rgba(0, 0, 0, 0.28);
  }

  .hero .btn--primary {
    padding: 0 25px;
    background: rgba(22, 35, 43, 0.28);
    border-color: rgba(255, 255, 255, 0.75);
    font-weight: 900;
  }

  .hero .btn--primary:hover {
    background: rgba(22, 35, 43, 0.48);
  }

  .about {
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
    display: none;
  }

  .about__responsive-title {
    margin-bottom: 0;
    font-size: clamp(28px, 7vw, 36px);
  }

  .about__content {
    max-width: 100%;
    padding-top: 0;
  }

  .about__content h2 {
    font-size: 22px;
  }

  .stats {
    flex-wrap: wrap;
    gap: 30px;
  }

  .stat__value {
    font-size: 26px;
  }

  .stat__plus {
    font-size: 18px;
  }

  blockquote {
    padding-inline-start: 16px;
    font-size: 18px;
  }

  .services {
    padding: 40px 0 60px;
  }

  .services__title-wrapper {
    justify-content: center;
  }

  .services__title-icon {
    gap: 14px;
  }

  .services__title-icon h2 {
    font-size: 28px;
  }

  .services__title-icon h2 br {
    display: none;
  }

  .services__arrows {
    display: none;
  }

  .service-card {
    flex-basis: 240px;
    width: 240px;
  }

  .case {
    padding: 50px 0 70px;
  }

  .case__grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
  }

  .case__sidebar h2 {
    margin: 0 0 24px;
    font-size: 34px;
  }

  .office {
    padding-bottom: 80px;
  }

  .cta {
    padding-bottom: 90px;
  }

  .cta__box {
    flex-direction: column;
    align-items: stretch;
    gap: 24px;
    padding: 28px 22px;
    text-align: center;
  }

  .cta__box .btn {
    align-self: center;
    width: 100%;
    max-width: 280px;
  }
}

/* =========================================================
   21. ≤576
   ========================================================= */
@media (max-width: 576px) {
  .footer__inner {
    grid-template-columns: 1fr;
    gap: 28px;
  }

  .map {
    height: 300px;
  }

  .contact-row {
    gap: 12px 24px;
  }

  .expertise {
    padding: 45px 0 75px;
  }

  .expertise::after {
    top: 180px;
    width: 430px;
    height: 430px;
    border-radius: 42px;
  }

  .expertise__responsive-title {
    margin-bottom: 22px;
    padding: 5px 8px 7px;
    font-size: 30px;
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

  .testimonials {
    padding: 32px 0 42px;
  }

  .testimonials__inner {
    width: 100%;
    max-width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .testimonials__title {
    position: relative;
    top: 0;
    left: auto;
    right: auto;
    width: 888px;
    height: 38px;
    margin: 0 auto -28px;
    flex: 0 0 138px;
  }

  .testimonial-diamond {
    width: 138px;
    height: 138px;
  }

  .testimonial-diamond h2 {
    width: 100px;
    margin: 0;
    font-size: 15px;
    line-height: 1.45;
  }

  .testimonial-quote {
    top: 67px;
    left: 18px;
    font-size: 32px;
    line-height: 1;
  }

  .testimonials__cards {
    width: 100%;
    max-width: 100%;
    margin: 0;
    min-width: 0;
  }

  .testimonials__track {
    width: max-content;
    gap: 14px;
    padding-inline: 20px;
  }

  .testimonial-card {
    flex: 0 0 min(300px, calc(100vw - 48px));
    width: min(300px, calc(100vw - 48px));
    min-width: 0;
    max-width: 300px;
  }

  .testimonial-card__box {
    height: 180px;
    padding: 16px 18px;
  }

  .testimonial-card__text {
    height: 110px;
  }

  .testimonial-card__stars {
    font-size: 16px;
  }

  .testimonial-card__client {
    gap: 10px;
    padding-top: 14px;
  }

  .testimonial-card__avatar {
    flex-basis: 46px;
    width: 46px;
    height: 46px;
  }

  .testimonial-card__client h3 {
    font-size: 15px;
  }

  .testimonial-card__client span {
    font-size: 11px;
  }

  .testimonial-dots {
    margin-top: 18px;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 7px;
  }
}

@media (max-width: 480px) {
  .case__grid {
    grid-template-columns: 1fr;
    grid-auto-rows: 170px;
  }

  .stats {
    gap: 20px;
  }

  .stat__value {
    font-size: 24px;
  }

  .stat__plus {
    font-size: 17px;
  }

  .cta h3 {
    font-size: 20px;
  }

  .service-card__media {
    height: 140px;
  }

  .service-card__body {
    padding: 16px 20px 20px;
  }

  .service-card__body h3 {
    font-size: 14px;
  }

  .service-card__body p {
    font-size: 12px;
  }
}

@media (max-width: 380px) {
  .header__inner .brand {
    inset-inline-start: 12px;
  }

  .header .brand__mark {
    flex-basis: 38px;
    width: 38px;
    height: 38px;
  }

  .header .brand__text {
    font-size: 23px;
  }
}

/* =========================================================
   22. FOOTER ≤420
   ========================================================= */
@media (max-width: 420px) {
  .footer {
    padding: 48px 0 30px;
    overflow: hidden;
  }

  .footer__inner {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    width: 100%;
    max-width: 100%;
    margin: 0;
    padding: 0 18px;
  }

  .footer__brand {
    margin-bottom: 28px;
    text-align: center;
  }

  .brand--footer {
    display: inline-flex;
    justify-content: center;
    max-width: 100%;
  }

  .brand--footer .brand__text {
    max-width: calc(100vw - 100px);
    font-size: 18px;
    white-space: normal;
    overflow-wrap: anywhere;
  }

  .footer__brand p {
    max-width: 100%;
    margin: 16px auto 12px;
    font-size: 12px;
    line-height: 1.8;
    overflow-wrap: anywhere;
  }

  .footer__brand small {
    display: block;
    font-size: 11px;
  }

  .footer-col {
    min-width: 0;
    border-top: 1px solid #e8eef3;
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
    margin-top: 0;
    padding: 0 4px;
    overflow: hidden;
    opacity: 0;
    pointer-events: none;
    transition:
      max-height 0.4s var(--ease),
      opacity 0.25s ease,
      padding 0.35s var(--ease);
  }

  .footer-col.is-open .footer-col__links {
    max-height: 300px;
    padding-bottom: 14px;
    opacity: 1;
    pointer-events: auto;
  }

  .footer-col__links a {
    margin: 0;
    padding: 9px 0;
    font-size: 12px;
    line-height: 1.6;
    overflow-wrap: anywhere;
  }

  .footer-col__links a:hover {
    transform: none;
  }

  .footer-social {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 0;
    padding-top: 28px;
  }

  .socials {
    flex-wrap: wrap;
    justify-content: center;
    gap: 12px;
    margin-bottom: 20px;
  }

  .socials a {
    flex: 0 0 32px;
    width: 32px;
    height: 32px;
  }

  .footer-social select {
    width: 100%;
    max-width: 240px;
    height: 40px;
    padding: 0 12px;
    font-size: 12px;
  }
}

/* =========================================================
   23. LOADER
   ========================================================= */
.ray-loader {
  position: fixed;
  inset: 0;
  z-index: 99999;
  overflow: hidden;
  background: #fff;
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
      #fff 0 8%,
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
  animation: ray-loader-glow 2.55s var(--ease) 0.08s both;
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
  animation: ray-loader-ambient 3s var(--ease) 0.02s both;
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
  will-change: transform, opacity, filter;
  animation: ray-piece-release 1.82s var(--ease) forwards;
}

.ray-piece--1 {
  background: linear-gradient(145deg, #eef3ff, #b9ccff 52%, #7299f2);
  clip-path: polygon(0 0, 100% 4%, 88% 100%, 7% 88%);
  --x: -44vw;
  --y: -38vh;
  --r: -9deg;
  animation-delay: 1.42s;
}

.ray-piece--2 {
  background: linear-gradient(145deg, #f6f3df, #e9d39a 55%, #c79a3b);
  clip-path: polygon(6% 0, 96% 8%, 100% 96%, 0 100%);
  --x: -15vw;
  --y: -41vh;
  --r: -4deg;
  animation-delay: 1.49s;
}

.ray-piece--3 {
  background: linear-gradient(145deg, #edf2ff, #b9c9f4 52%, #6d86c7);
  clip-path: polygon(4% 4%, 100% 0, 94% 100%, 0 90%);
  --x: 15vw;
  --y: -40vh;
  --r: 5deg;
  animation-delay: 1.56s;
}

.ray-piece--4 {
  background: linear-gradient(145deg, #e8faf7, #9edfd6 52%, #49aaa1);
  clip-path: polygon(0 6%, 92% 0, 100% 100%, 8% 94%);
  --x: 43vw;
  --y: -36vh;
  --r: 10deg;
  animation-delay: 1.63s;
}

.ray-piece--5 {
  background: linear-gradient(145deg, #e8f2fb, #9fc2e8 55%, #4d82bf);
  clip-path: polygon(0 0, 94% 8%, 100% 100%, 6% 92%);
  --x: -48vw;
  --y: 31vh;
  --r: -11deg;
  animation-delay: 1.6s;
}

.ray-piece--6 {
  background: linear-gradient(145deg, #eef1fb, #b7c1df 52%, #788db8);
  clip-path: polygon(6% 0, 100% 4%, 92% 100%, 0 94%);
  --x: -16vw;
  --y: 37vh;
  --r: -5deg;
  animation-delay: 1.67s;
}

.ray-piece--7 {
  background: linear-gradient(145deg, #f8f5ec, #e7d5ad 54%, #b98b42);
  clip-path: polygon(0 8%, 100% 0, 94% 94%, 8% 100%);
  --x: 16vw;
  --y: 38vh;
  --r: 6deg;
  animation-delay: 1.74s;
}

.ray-piece--8 {
  background: linear-gradient(145deg, #e9f4f8, #9acbd1 52%, #4e9ca4);
  clip-path: polygon(7% 0, 100% 8%, 92% 100%, 0 92%);
  --x: 44vw;
  --y: 32vh;
  --r: 11deg;
  animation-delay: 1.81s;
}

.ray-piece--9 {
  z-index: 2;
  width: 30vw;
  height: 30vw;
  background:
    radial-gradient(
      circle at 38% 34%,
      rgba(255, 255, 255, 0.96),
      transparent 34%
    ),
    linear-gradient(135deg, #eef3fb, #b9c8e8 38%, #8eb7d0 70%, #7db8aa);
  clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
  box-shadow:
    0 0 120px rgba(47, 84, 177, 0.16),
    0 0 70px rgba(42, 102, 214, 0.11),
    inset 0 0 44px rgba(255, 255, 255, 0.34);
  --x: 0;
  --y: 0;
  --r: 45deg;
  animation-delay: 0.08s;
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
  animation: ray-center-exit 0.82s var(--ease) 1.74s forwards;
}

.ray-loader__mark {
  position: relative;
  display: grid;
  place-items: center;
  width: 86px;
  height: 86px;
  border: 1px solid rgba(255, 255, 255, 0.7);
  border-radius: 21px;
  background: linear-gradient(
    145deg,
    #173b67,
    #285d9a 42%,
    #2b8f9f 72%,
    #6aaea0
  );
  transform: rotate(45deg) scale(0.68);
  box-shadow:
    0 22px 64px rgba(23, 59, 103, 0.22),
    0 0 0 11px rgba(255, 255, 255, 0.46),
    0 0 0 24px rgba(44, 91, 153, 0.07),
    0 0 46px rgba(0, 156, 150, 0.1);
  animation: ray-mark-in 1.05s var(--ease) 0.18s both;
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
  animation: ray-brand-in 0.94s var(--ease) 0.36s both;
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
  animation: ray-flash 1.12s var(--ease) 2.58s forwards;
}

.ray-loader-active,
.ray-loader-active body {
  overflow: hidden;
}

.ray-loader-leave-active {
  transition: opacity 0.42s var(--ease);
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
    transform: none;
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
</style>
