<template>
  <div class="app" :class="{ 'menu-open': menuOpen }">
    <div class="noise-layer" aria-hidden="true"></div>
    <div class="aurora-blue" aria-hidden="true"></div>

    <a class="skip-link" href="#main">{{ t("skipToMain") }}</a>

    <!-- Header -->
    <header class="header" :class="{ scrolled: isScrolled }">
      <nav class="nav container" :aria-label="t('mainNavAria')">
        <a
          class="brand"
          href="#home"
          :aria-label="t('brandAriaLabel')"
          @click.prevent="goTo('#home')"
        >
          <span class="brand-symbol" aria-hidden="true">
            <span class="brand-ring"></span>
            <span class="brand-dot"></span>
          </span>
          <span class="brand-copy">
            <strong>{{ t("brandName") }}</strong>
            <small>{{ t("brandSub") }}</small>
          </span>
        </a>

        <div
          id="nav-menu"
          ref="menuRef"
          class="nav-menu"
          :aria-hidden="menuOpen ? 'false' : 'true'"
        >
          <a
            v-for="link in navLinks"
            :key="link.href"
            :href="link.href"
            :aria-current="activeSection === link.id ? 'page' : undefined"
            :class="{ 'is-active': activeSection === link.id }"
            @click.prevent="goTo(link.href)"
          >
            {{ link.label }}
          </a>
        </div>

        <div class="nav-actions">
          <div
            class="lang-switch"
            role="group"
            :aria-label="t('langSwitchAria')"
          >
            <button
              type="button"
              class="icon-btn"
              :class="{ active: locale === 'fa' }"
              :aria-pressed="locale === 'fa'"
              title="فارسی"
              @click="switchLocale('fa')"
            >
              فا
            </button>

            <button
              type="button"
              class="icon-btn"
              :class="{ active: locale === 'en' }"
              :aria-pressed="locale === 'en'"
              title="English"
              @click="switchLocale('en')"
            >
              En
            </button>
          </div>

          <button
            type="button"
            class="icon-btn"
            :aria-label="t('toggleTheme')"
            :aria-pressed="theme === 'dark'"
            :title="t('toggleTheme')"
            @click="toggleTheme"
          >
            <span aria-hidden="true">{{ themeIcon }}</span>
          </button>

          <a
            href="#contact"
            class="btn btn-small btn-primary"
            @click.prevent="goTo('#contact')"
          >
            {{ t("startProject") }}
          </a>

          <button
            ref="hamburgerRef"
            class="hamburger"
            type="button"
            aria-haspopup="true"
            :aria-expanded="menuOpen"
            aria-controls="nav-menu"
            :aria-label="menuOpen ? t('menuClose') : t('menuOpenLabel')"
            @click="toggleMenu"
          >
            <span></span>
            <span></span>
          </button>
        </div>
      </nav>
    </header>

    <!-- Mobile menu backdrop -->
    <button
      v-if="menuOpen"
      class="nav-backdrop"
      type="button"
      :aria-label="t('menuClose')"
      @click="closeMenu"
    ></button>

    <main id="main">
      <!-- Hero -->
      <section id="home" class="hero section" aria-labelledby="hero-heading">
        <div class="container hero-grid">
          <div class="hero-content reveal">
            <p class="status-pill">
              <span aria-hidden="true"></span>
              {{ t("statusPill") }}
            </p>

            <h1 id="hero-heading" v-html="t('heroTitle')"></h1>

            <p class="hero-text">{{ t("heroText") }}</p>

            <div class="hero-actions">
              <a
                href="#services"
                class="btn btn-primary"
                @click.prevent="goTo('#services')"
              >
                {{ t("viewServices") }}
              </a>

              <a
                href="#about"
                class="btn btn-ghost"
                @click.prevent="goTo('#about')"
              >
                {{ t("aboutUs") }}
              </a>
            </div>

            <dl class="hero-stats">
              <div v-for="stat in stats" :key="stat.labelKey" class="stat-card">
                <dt class="visually-hidden">{{ t(stat.labelKey) }}</dt>
                <dd>
                  <strong>{{ stat.value }}</strong>
                  <span>{{ t(stat.labelKey) }}</span>
                </dd>
              </div>
            </dl>
          </div>

          <div
            class="hero-visual reveal"
            role="img"
            :aria-label="t('heroVisualAria')"
          >
            <div class="orbit orbit-one" aria-hidden="true"></div>
            <div class="orbit orbit-two" aria-hidden="true"></div>

            <div class="terminal-card" aria-hidden="true">
              <div class="terminal-top">
                <div class="dots">
                  <span></span>
                  <span></span>
                  <span></span>
                </div>
                <div class="terminal-title">raybod-pouye@ai</div>
              </div>

              <div class="terminal-body">
                <p><span class="muted">$</span> npm run deploy:network</p>
                <p><span class="success">✓</span> NLU intent detection</p>
                <p><span class="success">✓</span> RAG knowledge base</p>
                <p><span class="success">✓</span> LLM local inference</p>
                <p><span class="success">✓</span> Microservices ready</p>
                <p class="typing-line">
                  {{ t("terminalTyping") }}<span class="cursor"></span>
                </p>
              </div>

              <div class="terminal-footer">
                <span>{{ t("buildStatus") }}</span>
                <strong>{{ t("buildStatusValue") }}</strong>
              </div>
            </div>

            <div class="insight-card insight-top" aria-hidden="true">
              <span class="insight-icon">⚡</span>
              <div>
                <strong>16+</strong>
                <small>{{ t("yearsExp") }}</small>
              </div>
            </div>

            <div class="insight-card insight-bottom" aria-hidden="true">
              <span class="insight-icon">🛡️</span>
              <div>
                <strong>{{ t("secureBadge") }}</strong>
                <small>{{ t("secureBadgeSub") }}</small>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- About -->
      <section
        id="about"
        class="section about-section"
        aria-labelledby="about-heading"
      >
        <div class="container split-grid">
          <div class="section-intro reveal">
            <span class="section-label">{{ t("aboutUs") }}</span>
            <h2 id="about-heading" v-html="t('aboutHeading')"></h2>
          </div>

          <div class="about-copy reveal">
            <p>{{ t("aboutText") }}</p>

            <div class="about-points">
              <div
                v-for="point in aboutPoints"
                :key="point.key"
                class="about-point"
              >
                <span aria-hidden="true">{{ point.icon }}</span>
                <div>
                  <strong>{{ t(point.titleKey) }}</strong>
                  <p>{{ t(point.textKey) }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Services -->
      <section
        id="services"
        class="section services-section"
        aria-labelledby="services-heading"
      >
        <div class="container">
          <div class="center-heading reveal">
            <span class="section-label">{{ t("services") }}</span>
            <h2 id="services-heading">{{ t("servicesHeading") }}</h2>
            <p>{{ t("servicesText") }}</p>
          </div>

          <div class="services-grid">
            <article
              v-for="service in services"
              :key="service.key"
              class="service-card reveal"
            >
              <div class="service-icon" aria-hidden="true">
                {{ service.icon }}
              </div>
              <h3>{{ t(service.titleKey) }}</h3>
              <p>{{ t(service.descKey) }}</p>

              <ul>
                <li v-for="item in service.items" :key="item">{{ item }}</li>
              </ul>
            </article>
          </div>
        </div>
      </section>

      <!-- Why Us -->
      <section
        id="why"
        class="section why-section"
        aria-labelledby="why-heading"
      >
        <div class="container why-grid">
          <div class="why-panel reveal">
            <span class="section-label">{{ t("whyUs") }}</span>
            <h2 id="why-heading">{{ t("whyHeading") }}</h2>
            <p>{{ t("whyText") }}</p>
          </div>

          <div class="why-list">
            <article
              v-for="item in whyItems"
              :key="item.key"
              class="why-item reveal"
            >
              <span aria-hidden="true">{{ item.number }}</span>
              <div>
                <h3>{{ t(item.titleKey) }}</h3>
                <p>{{ t(item.textKey) }}</p>
              </div>
            </article>
          </div>
        </div>
      </section>

      <!-- Testimonials -->
      <section
        id="testimonials"
        class="section work-section"
        aria-labelledby="testimonials-heading"
      >
        <div class="container">
          <div class="center-heading reveal">
            <span class="section-label">{{ t("testimonials") }}</span>
            <h2 id="testimonials-heading">{{ t("testimonialsHeading") }}</h2>
            <p>{{ t("testimonialsText") }}</p>
          </div>

          <div class="work-grid">
            <article
              v-for="item in testimonials"
              :key="item.key"
              class="project-card reveal"
            >
              <div class="project-visual" aria-hidden="true">
                <span class="project-glow"></span>
                <div class="project-window">
                  <span></span>
                  <span></span>
                  <span></span>
                </div>
              </div>

              <div class="project-content">
                <blockquote class="testimonial-quote">
                  <h3>{{ t(item.titleKey) }}</h3>
                  <p>{{ t(item.descKey) }}</p>
                  <footer class="project-meta">
                    <cite>{{ item.role }}</cite>
                    <span>{{ item.org }}</span>
                  </footer>
                </blockquote>

                <div class="project-tags">
                  <span v-for="tag in item.tags" :key="tag">{{ tag }}</span>
                </div>
              </div>
            </article>
          </div>
        </div>
      </section>

      <!-- Search Features -->
      <section
        id="search-features"
        class="section process-section"
        aria-labelledby="search-features-heading"
      >
        <div class="container">
          <div class="center-heading reveal">
            <span class="section-label">{{ t("searchFeatures") }}</span>
            <h2 id="search-features-heading">
              {{ t("searchFeaturesHeading") }}
            </h2>
            <p>{{ t("searchFeaturesText") }}</p>
          </div>

          <div class="process-line process-line--five">
            <article
              v-for="(feat, idx) in searchFeatures"
              :key="idx"
              class="process-card reveal"
            >
              <span class="step-number" aria-hidden="true">
                {{ String(idx + 1).padStart(2, "0") }}
              </span>
              <h3>{{ t(feat.titleKey) }}</h3>
              <p>{{ t(feat.descKey) }}</p>
            </article>
          </div>
        </div>
      </section>

      <!-- Process -->
      <section
        id="process"
        class="section process-section"
        aria-labelledby="process-heading"
      >
        <div class="container">
          <div class="center-heading reveal">
            <span class="section-label">{{ t("process") }}</span>
            <h2 id="process-heading">{{ t("processHeading") }}</h2>
            <p>{{ t("processText") }}</p>
          </div>

          <ol class="process-line">
            <li
              v-for="(step, index) in processSteps"
              :key="step.key"
              class="process-card reveal"
            >
              <span class="step-number" aria-hidden="true">
                {{ String(index + 1).padStart(2, "0") }}
              </span>
              <h3>{{ t(step.titleKey) }}</h3>
              <p>{{ t(step.textKey) }}</p>
            </li>
          </ol>
        </div>
      </section>

      <!-- Articles -->
      <section
        id="articles"
        class="section about-section"
        aria-labelledby="articles-heading"
      >
        <div class="container">
          <div class="center-heading reveal">
            <span class="section-label">{{ t("articles") }}</span>
            <h2 id="articles-heading">{{ t("articlesHeading") }}</h2>
            <p>{{ t("articlesText") }}</p>
          </div>

          <div class="services-grid services-grid--three">
            <article
              v-for="(article, idx) in articles"
              :key="idx"
              class="service-card reveal"
            >
              <div class="service-icon" aria-hidden="true">📄</div>
              <h3>{{ t(article.titleKey) }}</h3>
              <p>{{ t(article.descKey) }}</p>
            </article>
          </div>
        </div>
      </section>

      <!-- Contact -->
      <section
        id="contact"
        class="section contact-section"
        aria-labelledby="contact-heading"
      >
        <div class="container">
          <div class="contact-card reveal">
            <div class="contact-copy">
              <span class="section-label">{{ t("startCollab") }}</span>
              <h2 id="contact-heading">{{ t("contactHeading") }}</h2>
              <p>{{ t("contactText") }}</p>

              <address class="contact-info">
                <a href="mailto:info@raybodpouye.ir">info@raybodpouye.ir</a>
                <a href="tel:+982128000000" dir="ltr">+98 21 2800 0000</a>
                <p class="contact-address">{{ t("address") }}</p>
              </address>
            </div>

            <form class="contact-form" novalidate @submit.prevent="submitForm">
              <label class="hp-field" aria-hidden="true">
                {{ t("hpLabel") }}
                <input
                  v-model="form.company"
                  type="text"
                  tabindex="-1"
                  autocomplete="off"
                />
              </label>

              <label>
                {{ t("yourName") }}
                <input
                  v-model.trim="form.name"
                  type="text"
                  :placeholder="t('namePlaceholder')"
                  autocomplete="name"
                  required
                  :aria-invalid="!!errors.name"
                  :aria-describedby="errors.name ? 'err-name' : undefined"
                  @blur="validateField('name')"
                />
                <span v-if="errors.name" id="err-name" class="field-error">
                  {{ errors.name }}
                </span>
              </label>

              <label>
                {{ t("email") }}
                <input
                  v-model.trim="form.email"
                  type="email"
                  :placeholder="t('emailPlaceholder')"
                  autocomplete="email"
                  required
                  :aria-invalid="!!errors.email"
                  :aria-describedby="errors.email ? 'err-email' : undefined"
                  @blur="validateField('email')"
                />
                <span v-if="errors.email" id="err-email" class="field-error">
                  {{ errors.email }}
                </span>
              </label>

              <label>
                {{ t("projectDesc") }}
                <textarea
                  v-model.trim="form.message"
                  rows="5"
                  :placeholder="t('messagePlaceholder')"
                  required
                  :aria-invalid="!!errors.message"
                  :aria-describedby="errors.message ? 'err-message' : undefined"
                  @blur="validateField('message')"
                ></textarea>
                <span
                  v-if="errors.message"
                  id="err-message"
                  class="field-error"
                >
                  {{ errors.message }}
                </span>
              </label>

              <button class="btn btn-primary" type="submit" :disabled="loading">
                <span
                  v-if="loading"
                  class="btn-spinner"
                  aria-hidden="true"
                ></span>
                {{ loading ? t("sending") : t("sendRequest") }}
              </button>

              <p
                v-if="formMessage"
                class="form-message"
                role="status"
                aria-live="polite"
              >
                {{ formMessage }}
              </p>
            </form>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="footer" :aria-label="t('footerAria')">
      <div class="container footer-grid">
        <div>
          <a
            class="brand footer-brand"
            href="#home"
            @click.prevent="goTo('#home')"
          >
            <span class="brand-symbol" aria-hidden="true">
              <span class="brand-ring"></span>
              <span class="brand-dot"></span>
            </span>
            <span class="brand-copy">
              <strong>{{ t("brandName") }}</strong>
              <small>{{ t("brandSub") }}</small>
            </span>
          </a>
          <p>{{ t("footerText") }}</p>
        </div>

        <nav class="footer-links" :aria-label="t('footerNavAria')">
          <a
            v-for="link in navLinks"
            :key="link.href"
            :href="link.href"
            @click.prevent="goTo(link.href)"
          >
            {{ link.label }}
          </a>
        </nav>
      </div>
    </footer>

    <!-- Back to top -->
    <transition name="fade">
      <button
        v-if="showBackToTop"
        class="back-to-top"
        type="button"
        :aria-label="t('backToTop')"
        @click="goTo('#home')"
      >
        ↑
      </button>
    </transition>
  </div>
</template>

<script setup>
import {
  ref,
  reactive,
  computed,
  onMounted,
  onBeforeUnmount,
  nextTick,
} from "vue";

/* =========================================================
   CONFIG — update these to match the real deployed domain
   ========================================================= */
const SITE_URL = "https://raybodpouye.ir";
const OG_IMAGE = `${SITE_URL}/og-cover.jpg`;

/* ---- SAFE ENV ---- */
const isClient =
  typeof window !== "undefined" && typeof document !== "undefined";

/* ---- STATE ---- */
const menuOpen = ref(false);
const isScrolled = ref(false);
const loading = ref(false);
const formMessage = ref("");
const activeSection = ref("home");
const showBackToTop = ref(false);
const hamburgerRef = ref(null);
const menuRef = ref(null);

const getInitialLocale = () => {
  if (!isClient) return "fa";
  const stored = localStorage.getItem("raybod-locale");
  if (stored === "fa" || stored === "en") return stored;
  return navigator.language?.toLowerCase().startsWith("fa") ? "fa" : "en";
};

const getInitialTheme = () => {
  if (!isClient) return "dark";
  const stored = localStorage.getItem("raybod-theme");
  if (stored === "light" || stored === "dark") return stored;
  return window.matchMedia?.("(prefers-color-scheme: light)").matches
    ? "light"
    : "dark";
};

const locale = ref(getInitialLocale());
const theme = ref(getInitialTheme());
const form = reactive({ name: "", email: "", message: "", company: "" });
const errors = reactive({ name: "", email: "", message: "" });
const currentYear = new Date().getFullYear();

/* ---- I18N ---- */
const translations = {
  fa: {
    skipToMain: "رفتن به محتوای اصلی",
    brandName: "رایبد پویه",
    brandAriaLabel: "رایبد پویه - صفحه اصلی",
    brandSub: "مشاوره و تولید نرم‌افزارهای هوشمند",
    mainNavAria: "ناوبری اصلی",
    menuOpenLabel: "باز کردن منو",
    menuClose: "بستن منو",
    langSwitchAria: "انتخاب زبان",
    toggleTheme: "تغییر تم رنگی",
    startProject: "شروع همکاری",
    statusPill: "پیشرو در مشاوره و تولید نرم‌افزارهای هوشمند شبکه",
    heroTitle:
      "راهکارهای نوآورانه برای <em>ارتباطات شبکه‌ای</em> و مدیریت دانش",
    heroText:
      "رایبد پویه با ۱۶ سال تجربه تخصصی و بیش از ۵۰ پروژه موفق، به سازمان‌ها کمک می‌کند تا با بهره‌گیری از هوش مصنوعی و فناوری‌های پیشرفته، عملکرد شبکه و دانش خود را به سطحی نوین ارتقا دهند.",
    viewServices: "مشاهده محصولات",
    aboutUs: "درباره ما",
    heroVisualAria: "نمایی از محیط توسعه هوشمند رایبد پویه با سرویس‌های فعال",
    techStripAria: "فناوری‌های مورد استفاده",
    statProjects: "پروژه موفق",
    statSatisfaction: "سازمان بزرگ",
    statQuality: "سال تجربه",
    aboutHeading: "ما فقط نرم‌افزار نمی‌سازیم؛<br />زیرساخت هوشمند می‌سازیم.",
    aboutText:
      "رایبد پویه شرکتی پیشرو در زمینه مشاوره و تولید نرم‌افزارهای هوشمند است که به توسعه و ارائه راهکارهای نوآورانه در زمینه انواع ارتباطات شبکه‌ای می‌پردازد. با بهره‌گیری از تکنولوژی‌های پیشرفته و تیم متخصص و متعهد، ما خدمات و محصولات هوشمندی ارائه می‌کنیم که به سازمان‌ها کمک می‌کند تا عملکردهای خود را بهبود ببخشند و به بهره‌وری بیشتری دست یابند.",
    aboutPoint1Title: "۱۶ سال تجربه تخصصی",
    aboutPoint1Text:
      "اجرا و پیاده‌سازی بیش از ۵۰ پروژه موفق و ارائه خدمات و محصولات به ۱۹ سازمان بزرگ دولتی و خصوصی.",
    aboutPoint2Title: "مدیریت هوشمند شبکه",
    aboutPoint2Text:
      "نرم‌افزار مدیریت هوشمند شبکه (ابری و on‑premise) با دستیار مجهز به NLU برای تشخیص intent و اجرای دستورات مدیریتی.",
    aboutPoint3Title: "هوش مصنوعی در خدمت دانش",
    aboutPoint3Text:
      "از NLU و LLMهای لوکال گرفته تا موتورهای جستجوی ترکیبی با RAG، تمامی راهکارها با آخرین فناوری‌های روز دنیا طراحی شده‌اند.",
    services: "محصولات و راهکارها",
    servicesHeading: "مجموعه‌ای از نرم‌افزارهای هوشمند برای سازمان شما",
    servicesText:
      "از مدیریت شبکه تا جستجوی هوشمند، از دانش تا چت‌بات‌های پیشرفته – همه با معماری مدرن و قابلیت استقرار on‑premise.",
    service1Title: "مدیریت هوشمند شبکه",
    service1Desc:
      "نرم‌افزار مدیریت هوشمند شبکه (ابری و on‑premise) شامل سرویس کنترل از راه دور شبکه و سرویس مدیریت دارایی‌های شبکه (Inventory) است که به یک دستیار هوشمند مجهز شده‌اند. دستیار هوشمند، انواع گزارشات از وضعیت شبکه را به‌طور خودکار تهیه و دستورات مدیریتی را اجرا می‌کند. در توسعه این دستیار از فناوری NLU برای تشخیص intent مورد نظر برای اجرا استفاده شده است.",
    service2Title: "موتور جستجوی هوشمند و دستیار GPT",
    service2Desc:
      "سرویس موتور جستجوی هوشمند و دستیار هوشمند مبتنی بر فناوری GPT توسعه یافته است و کلیه پایگاه‌های دانش درون سازمانی را به‌طور همزمان جستجو و پاسخ‌های ترکیبی تولید می‌کند. استفاده از زبان Go، فناوری NLP و سیستم‌های استخراج اطلاعات (RAG)، صرف زمان برای دستیابی به دانش را به حداقل رسانده است. این سرویس‌ها با LLMهای لوکال به‌صورت آفلاین و بدون نیاز به اینترنت یا سرویس ابری کار می‌کنند.",
    service3Title: "شبکه دانش هوشمند رایا",
    service3Desc:
      "شبکه دانش هوشمند رایا، نرم‌افزاری یکپارچه، هوشمند و دارای اپلیکیشن کراس پلتفرم است که تمرکز آن بر کاهش چرخه‌های زمانی و حداکثرسازی استفاده از منابع دانشی و خبرگان سازمان است. مبتنی بر معماری میکروسرویس، توزیع‌شده و کلود نیتیو، امکان مقیاس‌پذیری افقی برای حجم داده و کاربران بسیار بالا را فراهم می‌کند و به موتور جستجوی هوشمند و دستیار GPT مجهز است.",
    service4Title: "چت‌بات‌های هوشمند",
    service4Desc:
      "چت‌بات‌های هوشمند در سازمان‌ها و صنایع مختلف مستقر می‌شوند تا کارایی کارکنان را افزایش و هزینه‌ها را کاهش دهند؛ از پاسخگویی خودکار در خدمات مشتریان و میز کمک IT تا شناسایی مخاطبان هدف و تولید لید در فروش و بازاریابی. ترکیب NLU برای تشخیص intent و جستجوی هوشمند برای استخراج دانش، امکان سناریوهای پرسش‌وپاسخ چندمرحله‌ای را فراهم می‌کند.",
    service5Title: "خدمات مشاوره مدیریت دانش",
    service5Desc:
      "طراحی و پیاده‌سازی نظام مدیریت دانش بر اساس نیازهای واقعی سازمان، از شناسایی فرایندها تا استقرار راهکارهای متناسب با زیرساخت موجود.",
    service6Title: "راهکارهای اختصاصی",
    service6Desc:
      "از مسئله شروع می‌کنیم، با مشاوره و فناوری راه‌حل می‌سازیم. نیازهای سازمان را می‌شناسیم، راهکار مناسب را طراحی می‌کنیم و با تکیه بر فناوری‌های روز دنیا و هوش مصنوعی، آن را به راهکاری هوشمند، امن و متناسب با زیرساخت سازمان تبدیل می‌کنیم.",
    whyUs: "چرا رایبد پویه؟",
    whyHeading: "تمایز ما در هوشمندی، امنیت و بومی‌سازی است.",
    whyText:
      "ما با تکیه بر فناوری‌های پیشرفته و درک عمیق از نیازهای سازمان‌ها، راهکارهایی ارائه می‌دهیم که نه تنها کارآمد، بلکه قابل اعتماد و مطابق با زیرساخت بومی هستند.",
    whyItem1Title: "هوشمندسازی واقعی",
    whyItem1Text:
      "استفاده از NLU، RAG، LLMهای لوکال و الگوریتم‌های شبکه عصبی برای خودکارسازی و بهینه‌سازی فرایندها.",
    whyItem2Title: "امنیت و کنترل داده",
    whyItem2Text:
      "قابلیت استقرار on‑premise و استفاده از مدل‌های محلی بدون نیاز به سرویس‌های ابری خارجی، حریم داده‌ها را حفظ می‌کند.",
    whyItem3Title: "معماری مدرن و مقیاس‌پذیر",
    whyItem3Text:
      "محصولات مبتنی بر میکروسرویس، کلود نیتیو و توزیع شده که امکان رشد هم‌افقی با حجم داده و کاربران بالا را فراهم می‌کنند.",
    whyItem4Title: "طراحی مسئله‌محور",
    whyItem4Text:
      "فرایند پیاده‌سازی از شناخت دقیق چالش‌های سازمان شروع می‌شود، نه از انتخاب ابزار، تا خروجی کاملاً متناسب با نیاز باشد.",
    testimonials: "نظرات مشتریان",
    testimonialsHeading: "تجربه‌هایی که به ما اعتماد کرده‌اند",
    testimonialsText:
      "بخشی از بازخوردهای مدیران و کارشناسان سازمان‌های بزرگ درباره همکاری با رایبد پویه.",
    testimonial1Title: "مدیریت دانش از مسئله شروع شد",
    testimonial1Desc:
      "تفاوت اصلی این پروژه برای ما، شروع مدیریت دانش از مسئله‌های واقعی سازمان بود، نه از انتخاب ابزار و نرم‌افزار. تیم مشاور با شناخت دقیق فرایندها و چالش‌های سازمان، راهکارهایی متناسب با نیازهای ما طراحی و اجرا کرد.",
    testimonial2Title: "رویکرد اجرایی، نه فقط مستند",
    testimonial2Desc:
      "یکی از نقاط قوت همکاری، رویکرد کاملاً اجرایی تیم مشاور بود. خروجی پروژه فقط مجموعه‌ای از مستندات نبود؛ بلکه فرایندها، نقش‌ها و سازوکارهایی ایجاد شد که امکان ادامه و توسعه مدیریت دانش را در سازمان فراهم می‌کند.",
    testimonial3Title: "تشخیص سریع اختلالات شبکه",
    testimonial3Desc:
      "پیش از استفاده از سامانه، شناسایی منشأ اختلالات شبکه زمان زیادی از کارشناسان می‌گرفت. داشبوردها و اطلاعات متمرکز نرم‌افزار، فرآیند تشخیص و رفع مشکل را برای تیم ما بسیار سریع‌تر کرده است.",
    testimonial4Title: "امنیت و کنترل داده‌ها",
    testimonial4Desc:
      "برای ما امنیت و کنترل داده‌ها اهمیت بالایی داشت. استفاده از یک راهکار بومی که امکان استقرار در زیرساخت سازمان را فراهم می‌کند، باعث شد بتوانیم مدیریت و پایش شبکه را بدون وابستگی به سرویس‌های خارجی انجام دهیم.",
    testimonial5Title: "دسترسی سریع به اطلاعات",
    testimonial5Desc:
      "حجم بالای اسناد و اطلاعات سازمان باعث شده بود پیدا کردن اطلاعات موردنیاز زمان زیادی از کارشناسان بگیرد. موتور جستجو و دستیار هوشمند رایا این امکان را فراهم کرد که اطلاعات موردنیاز را سریع‌تر و دقیق‌تر پیدا کنیم.",
    testimonial6Title: "جستجوی مفهومی، نه کلمه‌ای",
    testimonial6Desc:
      "مزیت اصلی موتور جستجوی رایا برای ما این است که جستجو فقط بر اساس تطابق کلمات انجام نمی‌شود؛ سیستم می‌تواند مفهوم و ارتباط محتوای موردنظر را نیز درک کند و نتایج مرتبط‌تری ارائه دهد.",
    searchFeatures: "ویژگی‌های موتور جستجوی هوشمند رایا",
    searchFeaturesHeading: "جستجوی ترکیبی، دقیق و هوشمند",
    searchFeaturesText:
      "موتور جستجوی رایا با بهره‌گیری از جدیدترین روش‌ها، نتایج قوی‌تری ارائه می‌دهد.",
    searchFeature1Title: "استخراج کلمات/عبارات کلیدی",
    searchFeature1Desc:
      "استفاده از روش‌های BM25 و Inverted Index برای استخراج دقیق کلمات و عبارات کلیدی از متون.",
    searchFeature2Title: "جستجوی معنایی (Semantic Search)",
    searchFeature2Desc:
      "درک مفهوم و ارتباط محتوای موردنظر، فراتر از تطابق ساده کلمات.",
    searchFeature3Title: "جستجوی ترکیبی (Hybrid Search)",
    searchFeature3Desc:
      "ترکیب جستجوی کلیدواژه‌ای و معنایی برای دستیابی به بهترین و مرتبط‌ترین نتایج.",
    searchFeature4Title: "فیلتر و محدودسازی",
    searchFeature4Desc:
      "امکان محدود کردن جستجو به فیلدهای خاص و فیلتر کردن نتایج بر اساس فیلدهای مشخص.",
    searchFeature5Title: "پاسخ ترکیبی و Generative",
    searchFeature5Desc:
      "تولید پاسخ ترکیبی بر اساس نتایج با امکان اتصال به LLM لوکال به‌صورت آفلاین (GPT)",
    process: "فرآیند کار",
    processHeading: "از مسئله تا راهکار هوشمند",
    processText:
      "هر پروژه با شناخت عمیق نیازهای سازمان آغاز شده و با طراحی، پیاده‌سازی و استقرار، به نتیجه‌ای ملموس می‌رسد.",
    processStep1Title: "شناخت و مشاوره",
    processStep1Text:
      "بررسی فرایندها، چالش‌ها و اهداف سازمان برای تعریف دقیق راهکار.",
    processStep2Title: "طراحی معماری",
    processStep2Text:
      "انتخاب فناوری‌های مناسب، طراحی ساختار داده، رابط‌های کاربری و سرویس‌ها.",
    processStep3Title: "توسعه و یکپارچه‌سازی",
    processStep3Text:
      "پیاده‌سازی ماژول‌ها، اتصال به زیرساخت موجود و آموزش تیم‌های درگیر.",
    processStep4Title: "استقرار و پشتیبانی",
    processStep4Text:
      "نصب، راه‌اندازی، تست نهایی و ارائه پشتیبانی مستمر برای بهره‌برداری پایدار.",
    articles: "مقالات",
    articlesHeading: "دانش و بینش در حوزه هوش مصنوعی و شبکه",
    articlesText: "جدیدترین مطالب و مقالات تخصصی تیم رایبد پویه.",
    article1Title: "مدیریت دانش با رویکرد مسئله‌محور",
    article1Desc:
      "چگونه با شروع از چالش‌های واقعی، نظام مدیریت دانشی پایدار و اثربخش پیاده‌سازی کنیم.",
    article2Title: "مقایسه LLMهای لوکال و ابری در سازمان",
    article2Desc:
      "مزایا و معایب استقرار مدل‌های زبانی بزرگ به‌صورت on‑premise در مقابل سرویس‌های ابری.",
    article3Title: "معماری میکروسرویس در سیستم‌های دانش",
    article3Desc:
      "چگونه معماری توزیع‌شده و کلود نیتیو به مقیاس‌پذیری و انعطاف‌پذیری سیستم‌های دانش‌محور کمک می‌کند.",
    startCollab: "شروع همکاری",
    contactHeading: "آماده‌ایم تا سازمان شما را هوشمند کنیم.",
    contactText:
      "پروژه خود را با ما در میان بگذارید تا بهترین راهکار را متناسب با نیازهایتان طراحی و پیاده‌سازی کنیم.",
    yourName: "نام شما",
    namePlaceholder: "مثلاً علی رضایی",
    email: "ایمیل",
    emailPlaceholder: "you@example.com",
    projectDesc: "توضیحات پروژه",
    messagePlaceholder: "نیازها و چالش‌های خود را بنویسید...",
    hpLabel: "شرکت (این فیلد را خالی بگذارید)",
    sendRequest: "ارسال درخواست",
    sending: "در حال ارسال...",
    formSuccess: "درخواست شما ثبت شد. به‌زودی با شما تماس می‌گیریم.",
    formErrorGeneric: "لطفاً فیلدهای مشخص‌شده را بررسی کنید.",
    errNameRequired: "لطفاً نام خود را وارد کنید.",
    errEmailRequired: "لطفاً ایمیل خود را وارد کنید.",
    errEmailInvalid: "ایمیل واردشده معتبر نیست.",
    errMessageRequired: "لطفاً توضیحات پروژه را بنویسید.",
    errMessageShort: "توضیحات را کمی کامل‌تر بنویسید (حداقل ۱۰ نویسه).",
    address: "تهران، دانشگاه تهران، خیابان قدس، کوچه آذین، پلاک ۴",
    footerText:
      "رایبد پویه با ۱۶ سال تجربه و بیش از ۵۰ پروژه موفق، همراه مطمئن سازمان‌ها در مسیر هوشمندسازی، مدیریت شبکه و بهره‌وری دانش است.",
    footerAria: "پانویس سایت",
    footerNavAria: "لینک‌های پانویس",
    builtWith: "ساخته‌شده با Vue.js",
    backToTop: "بازگشت به بالای صفحه",
    terminalTyping: "استقرار سرویس‌های هوشمند",
    buildStatus: "وضعیت ساخت",
    buildStatusValue: "عالی",
    yearsExp: "سال تجربه",
    secureBadge: "امن",
    secureBadgeSub: "زیرساخت بومی",
    metaTitle: "رایبد پویه | مشاوره و تولید نرم‌افزارهای هوشمند شبکه",
    metaDesc:
      "رایبد پویه با ۱۶ سال تجربه و ۵۰+ پروژه موفق، راهکارهای هوشمند مدیریت شبکه و دانش را با فناوری‌هایی مانند NLU، RAG و LLMهای لوکال ارائه می‌دهد.",
    metaKeywords:
      "مدیریت هوشمند شبکه, موتور جستجوی هوشمند, مدیریت دانش سازمانی, چت‌بات هوشمند, RAG, NLU, LLM لوکال, رایبد پویه",
  },
  en: {
    skipToMain: "Skip to main content",
    brandName: "Raybod Pouye",
    brandAriaLabel: "Raybod Pouye - Home",
    brandSub: "Smart Software Consulting & Development",
    mainNavAria: "Main navigation",
    menuOpenLabel: "Open menu",
    menuClose: "Close menu",
    langSwitchAria: "Language selection",
    toggleTheme: "Toggle color theme",
    startProject: "Collaboration",
    statusPill: "Leader in smart networking & knowledge management",
    heroTitle:
      "Innovative solutions for <em>network communications</em> and knowledge management",
    heroText:
      "With 16 years of specialized expertise and over 50 successful projects, Raybod Pouye helps organizations leverage AI and advanced technologies to elevate network performance and knowledge assets to a new level.",
    viewServices: "View Products",
    aboutUs: "About Us",
    heroVisualAria:
      "A view of Raybod Pouye's intelligent development environment with active services",
    techStripAria: "Technologies we use",
    statProjects: "Successful Projects",
    statSatisfaction: "Enterprise Clients",
    statQuality: "Years of Experience",
    aboutHeading:
      "We don't just build software;<br />we build intelligent infrastructure.",
    aboutText:
      "Raybod Pouye is a leading company in smart software consulting and production, developing innovative solutions for all types of network communications. With advanced technologies and a committed expert team, we provide intelligent services and products that help organizations improve performance and achieve greater productivity.",
    aboutPoint1Title: "16 Years of Expertise",
    aboutPoint1Text:
      "Over 50 successful projects and services delivered to 19 major public and private organizations.",
    aboutPoint2Title: "Intelligent Network Management",
    aboutPoint2Text:
      "Cloud and on‑premise network management software with an AI assistant using NLU for intent recognition and automated command execution.",
    aboutPoint3Title: "AI for Knowledge",
    aboutPoint3Text:
      "From NLU and local LLMs to hybrid search engines with RAG, all solutions are designed with the latest cutting-edge technologies.",
    services: "Products & Solutions",
    servicesHeading: "A suite of intelligent software for your organization",
    servicesText:
      "From network management to intelligent search, from knowledge to advanced chatbots – all with modern architecture and on‑premise deployment capability.",
    service1Title: "Intelligent Network Management",
    service1Desc:
      "Cloud and on‑premise network management software including remote network control and inventory management, equipped with an AI assistant that automatically generates network status reports and executes management commands using NLU for intent detection.",
    service2Title: "Smart Search Engine & GPT Assistant",
    service2Desc:
      "An intelligent search engine and GPT‑based assistant that simultaneously searches all organizational knowledge bases and generates combined answers, built with Go, NLP, and RAG. All services run on local LLMs, installable on organizational servers without any internet or cloud dependency.",
    service3Title: "Raya Intelligent Knowledge Network",
    service3Desc:
      "Raya is an integrated, intelligent, cross‑platform knowledge network focused on reducing time cycles and maximizing the use of knowledge sources and experts within organizations. Built on microservices and cloud‑native architecture, it scales horizontally for high user counts and data volumes, and includes an intelligent search engine and GPT‑based assistant.",
    service4Title: "Smart Chatbots",
    service4Desc:
      "Intelligent chatbots deployed across industries to increase employee efficiency and reduce costs — from automated customer service and IT helpdesk to lead generation in sales and marketing. Combining NLU for intent detection with smart search, they support multi‑step conversation scenarios.",
    service5Title: "Knowledge Management Consulting",
    service5Desc:
      "Design and implementation of knowledge management systems based on real organizational needs, from process analysis to solution deployment tailored to existing infrastructure.",
    service6Title: "Custom Solutions",
    service6Desc:
      "We start with the problem, then build solutions through consulting and technology. We understand organizational needs, design the right approach, and leverage state‑of‑the‑art technologies and AI to create intelligent, secure, and infrastructure‑aligned solutions.",
    whyUs: "Why Raybod Pouye?",
    whyHeading:
      "Our differentiator is intelligence, security, and localization.",
    whyText:
      "By combining advanced technologies with a deep understanding of organizational needs, we deliver solutions that are not only efficient but also trustworthy and aligned with local infrastructure.",
    whyItem1Title: "Real Intelligence",
    whyItem1Text:
      "Using NLU, RAG, local LLMs, and neural network algorithms to automate and optimize processes.",
    whyItem2Title: "Data Security & Control",
    whyItem2Text:
      "On‑premise deployment and local models eliminate reliance on external cloud services, keeping your data private and secure.",
    whyItem3Title: "Modern & Scalable Architecture",
    whyItem3Text:
      "Products built on microservices, cloud‑native, and distributed designs that scale horizontally with high data and user volumes.",
    whyItem4Title: "Problem‑Driven Design",
    whyItem4Text:
      "Implementation starts with a thorough understanding of your organization's real challenges, not with tool selection, ensuring a perfectly tailored outcome.",
    testimonials: "Testimonials",
    testimonialsHeading: "Experiences of Those Who Trust Us",
    testimonialsText:
      "Feedback from managers and experts of major organizations about their collaboration with Raybod Pouye.",
    testimonial1Title: "Knowledge Management Started with Real Problems",
    testimonial1Desc:
      "The main difference for us was starting knowledge management from real organizational problems, not from tool selection. The consulting team, with precise understanding of our processes and challenges, designed and implemented solutions tailored to our needs.",
    testimonial2Title: "Execution‑Oriented, Not Just Documents",
    testimonial2Desc:
      "One of the strengths of this collaboration was the fully execution‑oriented approach. The project output wasn't just a set of documents; processes, roles, and mechanisms were created to enable the continuation of knowledge management in the organization.",
    testimonial3Title: "Faster Network Issue Diagnosis",
    testimonial3Desc:
      "Before using the system, identifying network issues took a lot of time. The centralized dashboards and information have made the diagnosis and resolution process much faster for our team.",
    testimonial4Title: "Data Security & Control",
    testimonial4Desc:
      "For us, data security and control were very important. Using a local solution that can be deployed on our infrastructure allowed us to manage and monitor the network without relying on external services.",
    testimonial5Title: "Fast Access to Information",
    testimonial5Desc:
      "The high volume of documents and information made finding the needed data time‑consuming. Raya's search engine and assistant enabled us to quickly and accurately find information across various organizational resources.",
    testimonial6Title: "Conceptual Search, Not Just Keywords",
    testimonial6Desc:
      "The main advantage of Raya's search engine is that it doesn't just match keywords; it understands the meaning and context, providing more relevant results.",
    searchFeatures: "Raya Smart Search Engine Features",
    searchFeaturesHeading: "Hybrid, Accurate & Intelligent Search",
    searchFeaturesText:
      "Raya's search engine leverages the newest methods to deliver stronger results.",
    searchFeature1Title: "Keyword/Phrase Extraction",
    searchFeature1Desc:
      "Using BM25 and Inverted Index for precise keyword and phrase extraction from texts.",
    searchFeature2Title: "Semantic Search",
    searchFeature2Desc:
      "Understanding the meaning and context beyond simple keyword matching.",
    searchFeature3Title: "Hybrid Search",
    searchFeature3Desc:
      "Combining keyword‑based and semantic search to achieve the best and most relevant results.",
    searchFeature4Title: "Filtering & Scoping",
    searchFeature4Desc:
      "Ability to restrict search to specific fields and filter results based on defined criteria.",
    searchFeature5Title: "Generative & Combined Answers",
    searchFeature5Desc:
      "Generating combined answers based on results, with connection to local LLM (GPT) offline.",
    process: "Process",
    processHeading: "From Problem to Intelligent Solution",
    processText:
      "Every project begins with a deep understanding of your needs and results in a tangible outcome through design, implementation, and deployment.",
    processStep1Title: "Discovery & Consulting",
    processStep1Text:
      "Analyzing processes, challenges, and goals to define the precise solution.",
    processStep2Title: "Architecture Design",
    processStep2Text:
      "Selecting appropriate technologies, designing data structures, interfaces, and services.",
    processStep3Title: "Development & Integration",
    processStep3Text:
      "Building modules, connecting to existing infrastructure, and training teams.",
    processStep4Title: "Deployment & Support",
    processStep4Text:
      "Installation, setup, final testing, and ongoing support for sustainable operation.",
    articles: "Articles",
    articlesHeading: "Knowledge & Insights in AI and Networking",
    articlesText: "Latest expert articles from the Raybod Pouye team.",
    article1Title: "Problem‑Driven Knowledge Management",
    article1Desc:
      "How to implement a sustainable and effective knowledge management system by starting from real organizational challenges.",
    article2Title: "Local vs. Cloud LLMs in Organizations",
    article2Desc:
      "Pros and cons of deploying large language models on‑premise versus using cloud services.",
    article3Title: "Microservices Architecture in Knowledge Systems",
    article3Desc:
      "How distributed and cloud‑native architecture contributes to scalability and flexibility of knowledge‑oriented systems.",
    startCollab: "Start Collaboration",
    contactHeading: "Ready to make your organization smarter.",
    contactText:
      "Share your project with us so we can design and implement the best solution for your needs.",
    yourName: "Your Name",
    namePlaceholder: "e.g. John Doe",
    email: "Email",
    emailPlaceholder: "you@example.com",
    projectDesc: "Project Description",
    messagePlaceholder: "Tell us about your needs and challenges...",
    hpLabel: "Company (leave this field empty)",
    sendRequest: "Send Request",
    sending: "Sending...",
    formSuccess: "Your request has been submitted. We'll contact you soon.",
    formErrorGeneric: "Please check the highlighted fields.",
    errNameRequired: "Please enter your name.",
    errEmailRequired: "Please enter your email.",
    errEmailInvalid: "That email address doesn't look valid.",
    errMessageRequired: "Please describe your project.",
    errMessageShort: "Please add a bit more detail (at least 10 characters).",
    address: "Tehran, University of Tehran, Qods St., Azin Alley, No. 4",
    footerText:
      "With 16 years of experience and over 50 successful projects, Raybod Pouye is a trusted partner for organizations on the path to intelligence, network management, and knowledge productivity.",
    footerAria: "Site footer",
    footerNavAria: "Footer links",
    builtWith: "Built with Vue.js",
    backToTop: "Back to top",
    terminalTyping: "Deploying intelligent services",
    buildStatus: "Build Status",
    buildStatusValue: "Excellent",
    yearsExp: "Years Experience",
    secureBadge: "Secure",
    secureBadgeSub: "Local Infrastructure",
    metaTitle: "Raybod Pouye | Smart Network Software Consulting & Development",
    metaDesc:
      "Raybod Pouye delivers intelligent network and knowledge management solutions using NLU, RAG, and local LLMs, with 16 years of expertise and 50+ successful projects.",
    metaKeywords:
      "intelligent network management, smart search engine, enterprise knowledge management, AI chatbot, RAG, NLU, local LLM, Raybod Pouye",
  },
};

const t = (key) => translations[locale.value]?.[key] || key;

/* ---- NAVIGATION ---- */
const navSectionIds = [
  "home",
  "about",
  "services",
  "why",
  "testimonials",
  "search-features",
  "process",
  "articles",
];

const navLinks = computed(() => {
  const labels = {
    fa: [
      "خانه",
      "درباره ما",
      "محصولات",
      "چرا ما",
      "نظرات",
      "ویژگی‌ها",
      "فرآیند",
      "مقالات",
    ],
    en: [
      "Home",
      "About",
      "Products",
      "Why Us",
      "Testimonials",
      "Features",
      "Process",
      "Articles",
    ],
  };
  return navSectionIds.map((id, i) => ({
    id,
    label: labels[locale.value][i],
    href: `#${id}`,
  }));
});

/* ---- STATIC DATA ---- */
const stats = computed(() => [
  { value: "16+", labelKey: "statQuality" },
  { value: "50+", labelKey: "statProjects" },
  { value: "19", labelKey: "statSatisfaction" },
]);

const aboutPoints = [
  {
    key: "p1",
    icon: "01",
    titleKey: "aboutPoint1Title",
    textKey: "aboutPoint1Text",
  },
  {
    key: "p2",
    icon: "02",
    titleKey: "aboutPoint2Title",
    textKey: "aboutPoint2Text",
  },
  {
    key: "p3",
    icon: "03",
    titleKey: "aboutPoint3Title",
    textKey: "aboutPoint3Text",
  },
];

const services = computed(() => [
  {
    key: "s1",
    icon: "✦",
    titleKey: "service1Title",
    descKey: "service1Desc",
    items: ["NLU Intent Detection", "Remote Control", "Asset Inventory"],
  },
  {
    key: "s2",
    icon: "⌘",
    titleKey: "service2Title",
    descKey: "service2Desc",
    items: ["Hybrid Search", "RAG", "Local LLM (GPT)"],
  },
  {
    key: "s3",
    icon: "◇",
    titleKey: "service3Title",
    descKey: "service3Desc",
    items: ["Microservices", "Cloud Native", "Horizontal Scaling"],
  },
  {
    key: "s4",
    icon: "⬡",
    titleKey: "service4Title",
    descKey: "service4Desc",
    items: ["Multi‑step Scenarios", "Intent Recognition", "Smart Search"],
  },
  {
    key: "s5",
    icon: "▣",
    titleKey: "service5Title",
    descKey: "service5Desc",
    items: ["Process Analysis", "Role Design", "Knowledge Strategy"],
  },
  {
    key: "s6",
    icon: "↗",
    titleKey: "service6Title",
    descKey: "service6Desc",
    items: ["Custom Development", "Localization", "Security"],
  },
]);

const whyItems = [
  {
    key: "w1",
    number: "01",
    titleKey: "whyItem1Title",
    textKey: "whyItem1Text",
  },
  {
    key: "w2",
    number: "02",
    titleKey: "whyItem2Title",
    textKey: "whyItem2Text",
  },
  {
    key: "w3",
    number: "03",
    titleKey: "whyItem3Title",
    textKey: "whyItem3Text",
  },
  {
    key: "w4",
    number: "04",
    titleKey: "whyItem4Title",
    textKey: "whyItem4Text",
  },
];

const testimonials = computed(() => {
  const meta = {
    fa: [
      ["مهندس علی کیانی", "فولاد مبارکه اصفهان"],
      ["مهندس رضا صادقی", "فولاد سنگان"],
      ["دکتر آرین مطاعی", "کتابخانه‌های عمومی کرمانشاه"],
      ["مدیر فناوری اطلاعات", "سازمان زیرساخت شبکه"],
      ["کارشناس ارشد دانش", "سازمان توسعه منابع"],
      ["مدیر مدیریت دانش", "سازمان صنعتی بزرگ"],
    ],
    en: [
      ["Ali Kiani", "Mobarakeh Steel Company"],
      ["Reza Sadeghi", "Sangān Steel Company"],
      ["Dr. Arian Motaee", "Kermanshah Public Libraries"],
      ["IT Director", "Network Infrastructure Org"],
      ["Senior Knowledge Analyst", "Resource Development Org"],
      ["Knowledge Management Lead", "Major Industrial Org"],
    ],
  };

  const tagSetsFa = [
    ["مدیریت دانش", "راهکار اختصاصی"],
    ["اجرا", "توسعه پایدار"],
    ["پایش شبکه", "داشبورد"],
    ["امنیت", "بومی‌سازی"],
    ["موتور جستجو", "دستیار هوشمند"],
    ["جستجوی مفهومی", "هوش مصنوعی"],
  ];

  const tagSetsEn = [
    ["Knowledge Management", "Custom Solution"],
    ["Execution", "Sustainable Development"],
    ["Network Monitoring", "Dashboard"],
    ["Security", "Localization"],
    ["Search Engine", "AI Assistant"],
    ["Semantic Search", "Artificial Intelligence"],
  ];

  const rows = meta[locale.value];
  const tagSets = locale.value === "fa" ? tagSetsFa : tagSetsEn;

  return [1, 2, 3, 4, 5, 6].map((n, i) => ({
    key: `t${n}`,
    role: rows[i][0],
    org: rows[i][1],
    titleKey: `testimonial${n}Title`,
    descKey: `testimonial${n}Desc`,
    tags: tagSets[i],
  }));
});

const searchFeatures = [1, 2, 3, 4, 5].map((n) => ({
  titleKey: `searchFeature${n}Title`,
  descKey: `searchFeature${n}Desc`,
}));

const processSteps = [1, 2, 3, 4].map((n) => ({
  key: `ps${n}`,
  titleKey: `processStep${n}Title`,
  textKey: `processStep${n}Text`,
}));

const articles = [1, 2, 3].map((n) => ({
  titleKey: `article${n}Title`,
  descKey: `article${n}Desc`,
}));

/* ---- THEME & LOCALE ---- */
const themeIcon = computed(() => (theme.value === "dark" ? "☀️" : "🌙"));

const applyThemeAttr = () => {
  if (!isClient) return;
  document.documentElement.setAttribute("data-theme", theme.value);
  const metaTheme = document.querySelector('meta[name="theme-color"]');
  if (metaTheme) {
    metaTheme.setAttribute(
      "content",
      theme.value === "dark" ? "#060914" : "#f8fafc",
    );
  }
};

const applyDocumentAttrs = () => {
  if (!isClient) return;
  document.documentElement.lang = locale.value;
  document.documentElement.dir = locale.value === "fa" ? "rtl" : "ltr";
  document.body.classList.toggle("ltr", locale.value === "en");
};

const switchLocale = async (lang) => {
  if (!isClient) return;
  if (locale.value === lang) return;

  locale.value = lang;
  localStorage.setItem("raybod-locale", lang);

  closeMenu();
  applyDocumentAttrs();
  updateSeoTags();

  await nextTick();
  sectionObserver?.disconnect();
  setupScrollSpy();
  revealObserver?.disconnect();
  setupReveal();
};

const toggleTheme = () => {
  if (!isClient) return;
  theme.value = theme.value === "dark" ? "light" : "dark";
  localStorage.setItem("raybod-theme", theme.value);
  applyThemeAttr();
};

/* ---- MENU & SCROLL ---- */
const openMenu = async () => {
  if (!isClient) return;
  menuOpen.value = true;
  document.body.style.overflow = "hidden";
  await nextTick();
  menuRef.value?.querySelector("a")?.focus();
};

const closeMenu = () => {
  if (!isClient) return;
  if (!menuOpen.value) return;
  menuOpen.value = false;
  document.body.style.overflow = "";
  hamburgerRef.value?.focus();
};

const toggleMenu = () => {
  if (menuOpen.value) closeMenu();
  else openMenu();
};

const goTo = (selector) => {
  if (!isClient) return;
  closeMenu();

  const el = document.querySelector(selector);
  if (!el) return;

  activeSection.value = selector.replace("#", "");
  el.scrollIntoView({ behavior: "smooth", block: "start" });
};

const handleScroll = () => {
  if (!isClient) return;
  isScrolled.value = window.scrollY > 20;
  showBackToTop.value = window.scrollY > window.innerHeight * 0.8;
};

const handleEscape = (e) => {
  if (e.key === "Escape") closeMenu();
};

const handleResizeMenu = () => {
  if (!isClient) return;
  if (window.innerWidth > 1120 && menuOpen.value) {
    closeMenu();
  }
};

const handleMenuKeydown = (e) => {
  if (!menuOpen.value || e.key !== "Tab" || !menuRef.value) return;

  const focusable = menuRef.value.querySelectorAll("a");
  if (!focusable.length) return;

  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  if (e.shiftKey && document.activeElement === first) {
    e.preventDefault();
    last.focus();
  } else if (!e.shiftKey && document.activeElement === last) {
    e.preventDefault();
    first.focus();
  }
};

/* ---- FORM VALIDATION ---- */
const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

const validateField = (field) => {
  if (field === "name") {
    errors.name = form.name.trim() ? "" : t("errNameRequired");
  } else if (field === "email") {
    if (!form.email.trim()) errors.email = t("errEmailRequired");
    else if (!emailPattern.test(form.email.trim())) {
      errors.email = t("errEmailInvalid");
    } else {
      errors.email = "";
    }
  } else if (field === "message") {
    if (!form.message.trim()) errors.message = t("errMessageRequired");
    else if (form.message.trim().length < 10) {
      errors.message = t("errMessageShort");
    } else {
      errors.message = "";
    }
  }
};

const validateAll = () => {
  validateField("name");
  validateField("email");
  validateField("message");
  return !errors.name && !errors.email && !errors.message;
};

const submitForm = async () => {
  if (loading.value) return;

  if (form.company) {
    formMessage.value = t("formSuccess");
    form.name = "";
    form.email = "";
    form.message = "";
    form.company = "";
    return;
  }

  if (!validateAll()) {
    formMessage.value = t("formErrorGeneric");
    return;
  }

  loading.value = true;
  formMessage.value = "";

  await new Promise((resolve) => setTimeout(resolve, 900));

  form.name = "";
  form.email = "";
  form.message = "";
  form.company = "";
  loading.value = false;
  formMessage.value = t("formSuccess");

  setTimeout(() => {
    formMessage.value = "";
  }, 4500);
};

/* ---- SEO ---- */
const ensureMeta = (attr, key, content) => {
  if (!isClient) return;
  let el = document.querySelector(`meta[${attr}="${key}"]`);
  if (!el) {
    el = document.createElement("meta");
    el.setAttribute(attr, key);
    document.head.appendChild(el);
  }
  el.setAttribute("content", content);
};

const ensureLink = (rel, href, extra = {}) => {
  if (!isClient) return;
  let selector = `link[rel="${rel}"]`;
  if (extra.hreflang) selector += `[hreflang="${extra.hreflang}"]`;

  let el = document.querySelector(selector);
  if (!el) {
    el = document.createElement("link");
    el.setAttribute("rel", rel);
    Object.entries(extra).forEach(([k, v]) => el.setAttribute(k, v));
    document.head.appendChild(el);
  }
  el.setAttribute("href", href);
};

const updateSeoTags = () => {
  if (!isClient) return;

  const title = t("metaTitle");
  const desc = t("metaDesc");
  const keywords = t("metaKeywords");
  const ogLocale = locale.value === "fa" ? "fa_IR" : "en_US";

  document.title = title;
  ensureMeta("name", "description", desc);
  ensureMeta("name", "keywords", keywords);
  ensureMeta("name", "robots", "index, follow");
  ensureMeta("name", "author", "Raybod Pouye");

  ensureMeta("property", "og:type", "website");
  ensureMeta("property", "og:title", title);
  ensureMeta("property", "og:description", desc);
  ensureMeta("property", "og:url", SITE_URL);
  ensureMeta("property", "og:image", OG_IMAGE);
  ensureMeta("property", "og:site_name", "Raybod Pouye");
  ensureMeta("property", "og:locale", ogLocale);

  ensureMeta("name", "twitter:card", "summary_large_image");
  ensureMeta("name", "twitter:title", title);
  ensureMeta("name", "twitter:description", desc);
  ensureMeta("name", "twitter:image", OG_IMAGE);

  ensureLink("canonical", SITE_URL);
  ensureLink("alternate", `${SITE_URL}/?lang=fa`, { hreflang: "fa" });
  ensureLink("alternate", `${SITE_URL}/?lang=en`, { hreflang: "en" });
  ensureLink("alternate", SITE_URL, { hreflang: "x-default" });

  const jsonLd = {
    "@context": "https://schema.org",
    "@graph": [
      {
        "@type": "Organization",
        "@id": `${SITE_URL}/#organization`,
        name: "Raybod Pouye",
        alternateName: "رایبد پویه",
        url: SITE_URL,
        logo: `${SITE_URL}/logo.png`,
        description: desc,
        email: "info@raybodpouye.ir",
        telephone: "+98-21-2800-0000",
        address: {
          "@type": "PostalAddress",
          streetAddress: "Qods St., Azin Alley, No. 4, University of Tehran",
          addressLocality: "Tehran",
          addressCountry: "IR",
        },
      },
      {
        "@type": "WebSite",
        "@id": `${SITE_URL}/#website`,
        url: SITE_URL,
        name: t("brandName"),
        inLanguage: locale.value === "fa" ? "fa-IR" : "en-US",
        publisher: { "@id": `${SITE_URL}/#organization` },
      },
    ],
  };

  let ld = document.getElementById("ld-json-org");
  if (!ld) {
    ld = document.createElement("script");
    ld.type = "application/ld+json";
    ld.id = "ld-json-org";
    document.head.appendChild(ld);
  }
  ld.textContent = JSON.stringify(jsonLd);
};

/* ---- SCROLLSPY ---- */
let sectionObserver = null;

const setupScrollSpy = () => {
  if (!isClient || !("IntersectionObserver" in window)) return;

  const sections = navSectionIds
    .map((id) => document.getElementById(id))
    .filter(Boolean);

  if (!sections.length) return;

  sectionObserver = new IntersectionObserver(
    (entries) => {
      const visibleEntries = entries.filter((entry) => entry.isIntersecting);
      if (!visibleEntries.length) return;

      const mostVisible = visibleEntries.sort(
        (a, b) => b.intersectionRatio - a.intersectionRatio,
      )[0];

      activeSection.value = mostVisible.target.id;
    },
    {
      rootMargin: "-35% 0px -45% 0px",
      threshold: [0.1, 0.2, 0.35, 0.5, 0.7],
    },
  );

  sections.forEach((section) => sectionObserver.observe(section));
};

/* ---- REVEAL ANIMATION ---- */
let revealObserver = null;

const setupReveal = () => {
  if (!isClient) return;

  const elements = document.querySelectorAll(".reveal");

  if (!("IntersectionObserver" in window)) {
    elements.forEach((el) => el.classList.add("visible"));
    return;
  }

  revealObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("visible");
          revealObserver?.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.14, rootMargin: "0px 0px -40px 0px" },
  );

  elements.forEach((el) => revealObserver.observe(el));
};

/* ---- LIFECYCLE ---- */
onMounted(async () => {
  if (!isClient) return;

  applyDocumentAttrs();
  applyThemeAttr();
  updateSeoTags();
  handleScroll();

  window.addEventListener("scroll", handleScroll, { passive: true });
  window.addEventListener("keydown", handleEscape);
  window.addEventListener("keydown", handleMenuKeydown);
  window.addEventListener("resize", handleResizeMenu, { passive: true });

  await nextTick();
  setupReveal();
  setupScrollSpy();
});

onBeforeUnmount(() => {
  if (!isClient) return;

  window.removeEventListener("scroll", handleScroll);
  window.removeEventListener("keydown", handleEscape);
  window.removeEventListener("keydown", handleMenuKeydown);
  window.removeEventListener("resize", handleResizeMenu);

  document.body.style.overflow = "";
  revealObserver?.disconnect();
  sectionObserver?.disconnect();
});
</script>

<style>
/* =========================================================
   RAYBOD POUYE – COMPLETE STYLESHEET
   No flash on resize, solid mobile menu, fixed subtitle
   ========================================================= */

/* ---------- Design Tokens ---------- */
:root {
  --ease-standard: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-smooth: cubic-bezier(0.16, 1, 0.3, 1);
  --container-max: 1240px;
  --container-pad: clamp(16px, 2vw, 28px);
  --header-h: 84px;
  --header-h-compact: 72px;
  --section-space: clamp(64px, 9vw, 128px);
  --radius-xs: 12px;
  --radius-sm: 16px;
  --radius-md: 22px;
  --radius-lg: 30px;
  --radius-xl: 40px;
  --radius-pill: 999px;
  --shadow-1: 0 10px 30px rgba(0, 0, 0, 0.12);
  --shadow-2: 0 18px 50px rgba(0, 0, 0, 0.18);
  --shadow-3: 0 30px 80px rgba(0, 0, 0, 0.24);
  --blur-soft: 14px;
  --blur-md: 20px;
  --blur-lg: 28px;

  --font-fa:
    "Vazirmatn", ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont,
    "Segoe UI", sans-serif;
  --font-en:
    "Inter", ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont,
    "Segoe UI", sans-serif;
  --font-body: var(--font-fa);
  --font-mono:
    "JetBrains Mono", "Fira Code", ui-monospace, SFMono-Regular, Menlo, Monaco,
    Consolas, monospace;

  --text-xs: clamp(0.72rem, 0.7rem + 0.2vw, 0.8rem);
  --text-sm: clamp(0.82rem, 0.78rem + 0.2vw, 0.9rem);
  --text-md: clamp(0.95rem, 0.9rem + 0.25vw, 1.05rem);
  --text-lg: clamp(1.05rem, 1rem + 0.35vw, 1.2rem);
  --text-xl: clamp(1.2rem, 1.05rem + 0.8vw, 1.6rem);
  --text-2xl: clamp(1.6rem, 1.15rem + 1.9vw, 2.9rem);
  --text-3xl: clamp(2.1rem, 1.3rem + 3.6vw, 5.4rem);
}

/* ---------- Dark Theme ---------- */
[data-theme="dark"] {
  --bg: #022d34;
  --bg-2: #012438;
  --surface: rgba(2, 45, 52, 0.72);
  --surface-2: rgba(1, 36, 56, 0.82);
  --surface-3: rgba(255, 255, 255, 0.045);
  --surface-4: rgba(255, 255, 255, 0.03);
  --text: #eaf2ff;
  --text-heading: #ffffff;
  --muted: #9fb0c7;
  --soft: #708198;
  --line: rgba(148, 163, 184, 0.16);
  --line-2: rgba(148, 163, 184, 0.26);
  --cyan: #2dd4ff;
  --blue: #5b7cfa;
  --violet: #8b5cf6;
  --green: #35f2a9;
  --danger: #ff5f7c;
  --warning: #ffd166;
  --btn-primary-text: #03121b;
  --btn-secondary-text: #ffffff;
  --terminal-bg: rgba(2, 45, 52, 0.88);
  --overlay-glow: 0 0 60px rgba(45, 212, 255, 0.14);
  --card-bg:
    linear-gradient(
      145deg,
      rgba(255, 255, 255, 0.06),
      rgba(255, 255, 255, 0.02)
    ),
    rgba(255, 255, 255, 0.025);
  --input-bg: rgba(255, 255, 255, 0.045);
  --input-focus-bg: rgba(255, 255, 255, 0.06);
  --grid-line: rgba(255, 255, 255, 0.03);
  --body-bg: var(--bg);
  --brand-symbol-bg:
    radial-gradient(
      circle at 30% 25%,
      rgba(53, 242, 169, 0.48),
      transparent 28%
    ),
    linear-gradient(135deg, rgba(45, 212, 255, 0.22), rgba(139, 92, 246, 0.22));
}

/* ---------- Light Theme ---------- */
[data-theme="light"] {
  --bg: #f7fafc;
  --bg-2: #f1f5f9;
  --surface: rgba(255, 255, 255, 0.76);
  --surface-2: rgba(248, 250, 252, 0.85);
  --surface-3: rgba(15, 23, 42, 0.035);
  --surface-4: rgba(15, 23, 42, 0.025);
  --text: #0f172a;
  --text-heading: #020617;
  --muted: #475569;
  --soft: #64748b;
  --line: rgba(100, 116, 139, 0.18);
  --line-2: rgba(100, 116, 139, 0.28);
  --cyan: #0891b2;
  --blue: #4f46e5;
  --violet: #7c3aed;
  --green: #059669;
  --danger: #e11d48;
  --warning: #f59e0b;
  --btn-primary-text: #ffffff;
  --btn-secondary-text: #0f172a;
  --terminal-bg: rgba(255, 255, 255, 0.92);
  --overlay-glow: 0 0 60px rgba(8, 145, 178, 0.08);
  --card-bg:
    linear-gradient(
      145deg,
      rgba(255, 255, 255, 0.95),
      rgba(248, 250, 252, 0.85)
    ),
    #ffffff;
  --input-bg: rgba(15, 23, 42, 0.035);
  --input-focus-bg: rgba(15, 23, 42, 0.05);
  --grid-line: rgba(0, 0, 0, 0.04);
  --body-bg: var(--bg);
  --brand-symbol-bg:
    radial-gradient(circle at 30% 25%, rgba(5, 150, 105, 0.4), transparent 28%),
    linear-gradient(135deg, rgba(8, 145, 178, 0.18), rgba(124, 58, 237, 0.18));
}

/* ---------- Global styles ---------- */
.about-section,
.why-section,
.process-section,
.footer {
  background: transparent !important;
}

.aurora-blue {
  position: fixed;
  inset: 0;
  z-index: -3;
  pointer-events: none;
  background: radial-gradient(
    circle at 50% 20%,
    var(--cyan) 0%,
    transparent 70%
  );
  opacity: 0.15;
  filter: blur(100px);
  animation: auroraPulse 8s ease-in-out infinite alternate;
}

@keyframes auroraPulse {
  0% {
    opacity: 0.12;
  }
  100% {
    opacity: 0.18;
  }
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  min-width: 320px;
  scroll-behavior: smooth;
  scroll-padding-top: calc(var(--header-h) + 18px);
  background: var(--bg);
  scrollbar-gutter: stable;
  overflow-x: hidden;
}

body {
  margin: 0;
  min-width: 320px;
  min-height: 100vh;
  overflow-x: clip;
  direction: rtl;
  background: var(--body-bg);
  color: var(--text);
  font-family: var(--font-body);
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  transition:
    background 0.35s var(--ease-smooth),
    color 0.35s var(--ease-smooth);
}

body.ltr {
  direction: ltr;
  font-family: var(--font-en);
}

img,
svg,
video,
canvas {
  display: block;
  max-width: 100%;
}
button,
input,
textarea,
select {
  font: inherit;
}
button {
  border: 0;
  background: none;
  cursor: pointer;
}
a {
  color: inherit;
  text-decoration: none;
}
ul,
ol {
  padding: 0;
  margin: 0;
  list-style: none;
}
p,
h1,
h2,
h3,
h4,
h5,
h6,
dd,
dt,
blockquote,
figure {
  margin: 0;
}

::selection {
  background: rgba(45, 212, 255, 0.22);
  color: #fff;
}
[data-theme="light"] ::selection {
  background: rgba(8, 145, 178, 0.18);
  color: #020617;
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

body::before {
  content: "";
  position: fixed;
  inset: 0;
  z-index: -5;
  pointer-events: none;
  background-image:
    linear-gradient(var(--grid-line) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid-line) 1px, transparent 1px);
  background-size: 54px 54px;
  mask-image: radial-gradient(circle at 50% 16%, #000 0%, transparent 68%);
}

.app {
  position: relative;
  min-height: 100vh;
  overflow: clip;
  isolation: isolate;
}

.noise-layer {
  position: fixed;
  inset: 0;
  z-index: -2;
  pointer-events: none;
  opacity: 0.1;
  background-image:
    radial-gradient(
      circle at 25% 25%,
      rgba(128, 128, 128, 0.8) 0 1px,
      transparent 1px
    ),
    radial-gradient(
      circle at 75% 75%,
      rgba(128, 128, 128, 0.38) 0 1px,
      transparent 1px
    );
  background-size:
    40px 40px,
    62px 62px;
  mix-blend-mode: overlay;
}
[data-theme="light"] .noise-layer {
  opacity: 0.05;
}

.skip-link {
  position: fixed;
  top: 14px;
  inset-inline-end: 14px;
  z-index: 9999;
  padding: 12px 16px;
  border-radius: var(--radius-pill);
  background: var(--cyan);
  color: #02111a;
  font-weight: 900;
  transform: translateY(-150%);
  transition: transform 0.25s var(--ease-standard);
}
.skip-link:focus {
  transform: translateY(0);
}

/* ---------- Layout ---------- */
.container {
  width: min(var(--container-max), calc(100% - (var(--container-pad) * 2)));
  margin-inline: auto;
}
.section {
  position: relative;
  padding-block: var(--section-space);
}

.section-label,
.status-pill {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  width: fit-content;
  min-height: 36px;
  padding-inline: 12px;
  border: 1px solid rgba(45, 212, 255, 0.2);
  border-radius: var(--radius-pill);
  background: rgba(45, 212, 255, 0.06);
  color: var(--cyan);
  font-size: var(--text-sm);
  font-weight: 900;
  letter-spacing: 0.01em;
}
.status-pill span {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--green);
  animation: pulse 1.8s infinite;
}

.section-title,
.center-heading h2,
.section-intro h2,
.why-panel h2,
.contact-copy h2 {
  color: var(--text-heading);
  font-size: var(--text-2xl);
  line-height: 1.12;
  letter-spacing: -0.03em;
  font-weight: 900;
}

.center-heading p,
.about-copy > p,
.why-panel > p,
.contact-copy > p {
  margin-top: 16px;
  color: var(--muted);
  font-size: var(--text-md);
  line-height: 2;
  font-weight: 500;
}

.center-heading {
  max-width: 760px;
  margin-inline: auto;
  margin-bottom: 52px;
  text-align: center;
}
.center-heading .section-label {
  margin-inline: auto;
}

/* LTR overrides */
body.ltr .brand,
body.ltr .nav-menu a,
body.ltr .btn,
body.ltr .btn-small,
body.ltr .icon-btn,
body.ltr .section-label,
body.ltr .status-pill,
body.ltr input,
body.ltr textarea {
  font-family: var(--font-en);
}

body.ltr .brand-copy strong {
  font-size: 0.9rem;
  font-weight: 800;
  letter-spacing: -0.01em;
}
body.ltr .brand-copy small {
  font-size: 0.66rem;
  font-weight: 700;
  letter-spacing: 0.01em;
}
body.ltr .nav-menu a {
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: -0.01em;
}
body.ltr .btn,
body.ltr .btn-small {
  font-size: 0.84rem;
  font-weight: 800;
  letter-spacing: -0.01em;
}
body.ltr .hero h1 {
  letter-spacing: -0.03em;
  line-height: 1.04;
  font-weight: 900;
}
body.ltr .section-title,
body.ltr .center-heading h2,
body.ltr .section-intro h2,
body.ltr .why-panel h2,
body.ltr .contact-copy h2 {
  letter-spacing: -0.02em;
  font-weight: 850;
}
body.ltr .hero-text,
body.ltr .center-heading p,
body.ltr .about-copy > p,
body.ltr .why-panel > p,
body.ltr .contact-copy > p,
body.ltr .service-card p,
body.ltr .why-item p,
body.ltr .testimonial-quote p,
body.ltr .process-card p,
body.ltr .footer p {
  font-size: 0.96rem;
  line-height: 1.9;
}

/* ---------- Header & Nav ---------- */
.header {
  position: fixed;
  inset: 0 0 auto 0;
  z-index: 1000;
  height: var(--header-h);
  border-bottom: 1px solid transparent;
  transition:
    height 0.25s var(--ease-standard),
    background 0.25s var(--ease-standard),
    border-color 0.25s var(--ease-standard),
    box-shadow 0.25s var(--ease-standard);
}
.header.scrolled {
  height: var(--header-h-compact);
  border-bottom-color: var(--line);
  background: var(--surface);
  box-shadow: 0 12px 45px rgba(0, 0, 0, 0.14);
  backdrop-filter: blur(var(--blur-lg));
}
[data-theme="light"] .header.scrolled {
  background: rgba(255, 255, 255, 0.82);
  box-shadow: 0 12px 45px rgba(0, 0, 0, 0.06);
}

.nav {
  height: 100%;
  display: grid;
  grid-template-columns: auto minmax(0, 1fr) auto;
  align-items: center;
  gap: 16px;
  min-width: 0;
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  width: fit-content;
  min-width: 0;
  max-width: 100%;
  flex-shrink: 0;
}
.brand-symbol {
  position: relative;
  display: grid;
  place-items: center;
  width: 46px;
  height: 46px;
  border-radius: 16px;
  border: 1px solid rgba(45, 212, 255, 0.25);
  background: var(--brand-symbol-bg);
  box-shadow: 0 0 28px rgba(45, 212, 255, 0.1);
  overflow: hidden;
  flex-shrink: 0;
}
.brand-symbol::before {
  content: "";
  position: absolute;
  inset: -45%;
  background: conic-gradient(
    from 180deg,
    transparent,
    rgba(45, 212, 255, 0.55),
    transparent,
    rgba(139, 92, 246, 0.5),
    transparent
  );
  animation: spin 7s linear infinite;
}
.brand-ring {
  position: relative;
  z-index: 1;
  width: 24px;
  height: 24px;
  border: 1px solid rgba(255, 255, 255, 0.7);
  border-radius: 50%;
}
.brand-ring::before {
  content: "";
  position: absolute;
  top: 3px;
  inset-inline-end: -3px;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #fff;
  box-shadow: 0 0 14px #fff;
}
.brand-dot {
  position: absolute;
  z-index: 2;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--green);
  box-shadow: 0 0 16px rgba(53, 242, 169, 0.7);
}
.brand-copy {
  display: grid;
  gap: 2px;
  line-height: 1.08;
  min-width: 0;
  overflow: visible; /* <-- FIX subtitle clipping */
  padding-block: 1px;
}
.brand-copy strong {
  color: var(--text-heading);
  font-size: 0.95rem;
  font-weight: 900;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.brand-copy small {
  color: var(--soft);
  font-size: 0.7rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  line-height: 1.5; /* <-- enough height for Persian letters */
}

/* Desktop pill nav (unchanged) */
.nav-menu {
  justify-self: center;
  min-width: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
  padding: 5px;
  border: 1px solid var(--line);
  border-radius: var(--radius-pill);
  background: var(--surface-4);
  backdrop-filter: blur(var(--blur-soft));
}
.nav-menu a {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 38px;
  padding-inline: 14px;
  border-radius: var(--radius-pill);
  color: var(--muted);
  font-size: 0.85rem;
  font-weight: 800;
  white-space: nowrap;
  transition:
    color 0.22s var(--ease-standard),
    background 0.22s var(--ease-standard),
    transform 0.22s var(--ease-standard);
}
.nav-menu a:hover,
.nav-menu a:focus-visible {
  color: var(--text);
  background: var(--surface-3);
  transform: translateY(-1px);
}
.nav-menu a.is-active,
.nav-menu a[aria-current="page"] {
  color: var(--cyan);
  background: rgba(45, 212, 255, 0.1);
}

.nav-actions {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 8px;
  min-width: 0;
  flex-shrink: 0;
  white-space: nowrap;
}
.nav-actions > * {
  flex-shrink: 0;
}

.lang-switch {
  display: inline-flex;
  gap: 4px;
  flex-shrink: 0;
}

.icon-btn {
  display: grid;
  place-items: center;
  width: 42px;
  height: 42px;
  border-radius: 14px;
  border: 1px solid var(--line);
  background: var(--surface-3);
  color: var(--muted);
  font-size: 1rem;
  transition:
    color 0.22s var(--ease-standard),
    background 0.22s var(--ease-standard),
    border-color 0.22s var(--ease-standard),
    transform 0.22s var(--ease-standard);
}
.icon-btn:hover,
.icon-btn:focus-visible {
  color: var(--text);
  border-color: var(--line-2);
  background: var(--surface-2);
  transform: translateY(-1px);
}
.icon-btn.active {
  color: var(--cyan);
  border-color: rgba(45, 212, 255, 0.34);
  background: rgba(45, 212, 255, 0.08);
}

/* ---------- Buttons ---------- */
.btn {
  position: relative;
  isolation: isolate;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  min-height: 50px;
  padding-inline: 20px;
  border-radius: var(--radius-pill);
  border: 1px solid transparent;
  overflow: hidden;
  font-size: 0.92rem;
  font-weight: 900;
  white-space: nowrap;
  transition:
    transform 0.25s var(--ease-standard),
    box-shadow 0.25s var(--ease-standard),
    border-color 0.25s var(--ease-standard),
    background 0.25s var(--ease-standard),
    color 0.25s var(--ease-standard);
}
.btn::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  translate: 115% 0;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.24),
    transparent
  );
  transition: translate 0.55s var(--ease-standard);
}
.btn:hover::before {
  translate: -115% 0;
}
.btn:hover,
.btn:focus-visible {
  transform: translateY(-2px);
}
.btn:disabled {
  opacity: 0.65;
  cursor: not-allowed;
  transform: none;
}

.btn-primary {
  border-color: rgba(45, 212, 255, 0.34);
  background:
    radial-gradient(
      circle at 30% 0%,
      rgba(255, 255, 255, 0.28),
      transparent 32%
    ),
    linear-gradient(135deg, var(--cyan), var(--blue) 55%, var(--violet));
  color: var(--btn-primary-text);
  box-shadow:
    0 16px 44px rgba(45, 212, 255, 0.16),
    inset 0 1px 0 rgba(255, 255, 255, 0.24);
}
.btn-ghost {
  border-color: var(--line-2);
  background: var(--surface-3);
  color: var(--btn-secondary-text);
  backdrop-filter: blur(var(--blur-soft));
}
.btn-ghost:hover,
.btn-ghost:focus-visible {
  border-color: rgba(45, 212, 255, 0.35);
  box-shadow: var(--shadow-1);
}
.btn-small {
  min-height: 40px;
  padding-inline: 16px;
  font-size: 0.82rem;
}
.btn-spinner {
  width: 15px;
  height: 15px;
  border-radius: 50%;
  border: 2px solid rgba(0, 0, 0, 0.25);
  border-top-color: currentColor;
  animation: spin 0.7s linear infinite;
}

/* ---------- Hero ---------- */
.hero {
  min-height: 100svh;
  display: grid;
  align-items: center;
  padding-top: calc(var(--header-h) + 52px);
  overflow: clip;
}
.hero-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.05fr) minmax(380px, 0.95fr);
  align-items: center;
  gap: clamp(28px, 5vw, 68px);
}
.hero h1 {
  max-width: 860px;
  margin-top: 22px;
  color: var(--text-heading);
  font-size: var(--text-3xl);
  line-height: 1.02;
  letter-spacing: -0.04em;
  font-weight: 950;
}
.hero h1 em {
  font-style: normal;
  background: linear-gradient(
    135deg,
    #fff,
    var(--cyan) 32%,
    var(--green) 58%,
    var(--violet)
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
[data-theme="light"] .hero h1 em {
  background: linear-gradient(
    135deg,
    #020617,
    var(--cyan) 32%,
    var(--green) 58%,
    var(--violet)
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
.hero-text {
  max-width: 720px;
  margin-top: 18px;
  color: var(--muted);
  font-size: var(--text-md);
  line-height: 2;
  font-weight: 500;
}
.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 30px;
}
.hero-stats {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 12px;
  max-width: 620px;
  margin-top: 36px;
  margin-inline: 0;
}
.stat-card {
  position: relative;
  overflow: hidden;
  padding: 18px;
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  background: var(--card-bg);
  backdrop-filter: blur(var(--blur-soft));
  box-shadow: var(--shadow-1);
}
.stat-card dd {
  margin: 0;
}
.stat-card strong {
  display: block;
  margin-bottom: 4px;
  color: var(--text-heading);
  font-size: clamp(1.15rem, 1rem + 0.8vw, 1.42rem);
  font-weight: 1000;
}
.stat-card span {
  color: var(--soft);
  font-size: 0.84rem;
  font-weight: 800;
}

/* Hero visual & terminal */
.hero-visual {
  position: relative;
  min-height: 580px;
  display: grid;
  place-items: center;
}
.orbit {
  position: absolute;
  border: 1px solid rgba(45, 212, 255, 0.15);
  border-radius: 50%;
  pointer-events: none;
}
.orbit-one {
  width: min(92%, 500px);
  aspect-ratio: 1;
  animation: spin 26s linear infinite;
}
.orbit-two {
  width: min(66%, 350px);
  aspect-ratio: 1;
  border-color: rgba(139, 92, 246, 0.2);
  animation: spin 18s linear infinite reverse;
}
.terminal-card {
  position: relative;
  z-index: 2;
  width: min(100%, 540px);
  border: 1px solid rgba(148, 163, 184, 0.2);
  border-radius: 30px;
  overflow: hidden;
  background: var(--terminal-bg);
  box-shadow: var(--shadow-3), var(--overlay-glow);
  backdrop-filter: blur(var(--blur-lg));
}
.terminal-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 14px;
  padding: 16px;
  border-bottom: 1px solid var(--line);
  background: var(--surface-3);
}
.dots {
  display: flex;
  gap: 6px;
}
.dots span {
  width: 11px;
  height: 11px;
  border-radius: 50%;
}
.dots span:nth-child(1) {
  background: var(--danger);
}
.dots span:nth-child(2) {
  background: var(--warning);
}
.dots span:nth-child(3) {
  background: var(--green);
}
.terminal-title {
  color: var(--soft);
  font-family: var(--font-mono);
  font-size: 0.78rem;
  font-weight: 700;
}
.terminal-body {
  text-align: left;
  padding: 22px;
  font-family: var(--font-mono);
}
.terminal-body p {
  margin-bottom: 11px;
  color: #dbeafe;
  font-size: clamp(0.72rem, 1.1vw, 0.86rem);
  line-height: 1.75;
}
[data-theme="light"] .terminal-body p {
  color: #1e293b;
}
.muted {
  color: #64748b;
}
.success {
  color: var(--green);
}
.typing-line {
  color: var(--cyan) !important;
}
.cursor {
  display: inline-block;
  width: 8px;
  height: 1em;
  margin-left: 4px;
  vertical-align: -0.15em;
  background: var(--cyan);
  animation: blink 0.9s steps(2) infinite;
}
.terminal-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 14px;
  padding: 14px 16px;
  border-top: 1px solid var(--line);
  background: var(--surface-3);
}
.terminal-footer span {
  color: var(--soft);
  font-size: 0.84rem;
  font-weight: 800;
}
.terminal-footer strong {
  color: var(--green);
  font-weight: 1000;
}

/* Insight cards */
.insight-card {
  position: absolute;
  z-index: 3;
  display: flex;
  align-items: center;
  gap: 10px;
  width: 180px;
  padding: 12px;
  border: 1px solid var(--line);
  border-radius: 20px;
  background: var(--terminal-bg);
  box-shadow: var(--shadow-2);
  backdrop-filter: blur(var(--blur-soft));
  animation: float 6s var(--ease-smooth) infinite;
}
.insight-icon {
  display: grid;
  place-items: center;
  width: 40px;
  height: 40px;
  border-radius: 14px;
  background: rgba(45, 212, 255, 0.08);
  color: var(--cyan);
  font-size: 1.05rem;
}
.insight-card strong {
  display: block;
  color: var(--text-heading);
  font-size: 0.94rem;
  font-weight: 900;
}
.insight-card small {
  color: var(--soft);
  font-size: 0.72rem;
  font-weight: 800;
}
.insight-top {
  top: 60px;
  inset-inline-end: -4px;
}
.insight-bottom {
  bottom: 60px;
  inset-inline-start: -4px;
  animation-delay: -2.5s;
}

/* ---------- Split Grids ---------- */
.split-grid {
  display: grid;
  grid-template-columns: minmax(0, 0.92fr) minmax(320px, 1.08fr);
  gap: clamp(24px, 5vw, 60px);
  align-items: start;
}
.why-grid {
  display: grid;
  grid-template-columns: minmax(320px, 0.92fr) minmax(0, 1.08fr);
  gap: clamp(24px, 4vw, 48px);
  align-items: start;
}
.about-copy {
  padding: clamp(18px, 3vw, 28px);
}
.about-points {
  display: grid;
  gap: 12px;
  margin-top: 24px;
}
.about-point {
  display: flex;
  gap: 12px;
  padding: 14px;
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  background: var(--surface-3);
}
.about-point > span {
  display: grid;
  place-items: center;
  flex: 0 0 38px;
  width: 38px;
  height: 38px;
  border-radius: 14px;
  border: 1px solid rgba(45, 212, 255, 0.22);
  background: rgba(45, 212, 255, 0.06);
  color: var(--cyan);
  font-size: 0.84rem;
  font-weight: 1000;
}
.about-point strong {
  display: block;
  margin-bottom: 4px;
  color: var(--text-heading);
  font-size: 0.96rem;
  font-weight: 950;
}
.about-point p {
  color: var(--muted);
  line-height: 1.8;
  font-size: 0.88rem;
}

/* ---------- Service Cards ---------- */
.services-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 16px;
}
.services-grid--three {
  grid-template-columns: repeat(3, minmax(0, 1fr));
}
.service-card {
  position: relative;
  min-height: 300px;
  overflow: hidden;
  padding: 22px;
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  background: var(--card-bg);
  box-shadow: var(--shadow-1);
  backdrop-filter: blur(var(--blur-soft));
  transition:
    transform 0.28s var(--ease-standard),
    border-color 0.28s var(--ease-standard),
    box-shadow 0.28s var(--ease-standard);
}
.service-card::before {
  content: "";
  position: absolute;
  inset: 0;
  opacity: 0;
  background:
    radial-gradient(
      circle at 20% 0%,
      rgba(45, 212, 255, 0.12),
      transparent 35%
    ),
    radial-gradient(circle at 90% 20%, rgba(139, 92, 246, 0.1), transparent 34%);
  transition: opacity 0.28s var(--ease-standard);
}
.service-card:hover {
  transform: translateY(-7px);
  border-color: rgba(45, 212, 255, 0.3);
  box-shadow: var(--shadow-2), var(--overlay-glow);
}
.service-card:hover::before {
  opacity: 1;
}
.service-icon {
  position: relative;
  z-index: 1;
  display: grid;
  place-items: center;
  width: 48px;
  height: 48px;
  margin-bottom: 20px;
  border-radius: 16px;
  border: 1px solid rgba(45, 212, 255, 0.2);
  background: rgba(45, 212, 255, 0.06);
  color: var(--cyan);
  font-size: 1.15rem;
  font-weight: 1000;
}
.service-card h3 {
  position: relative;
  z-index: 1;
  margin-bottom: 10px;
  color: var(--text-heading);
  font-size: 1.14rem;
  font-weight: 1000;
}
.service-card p {
  position: relative;
  z-index: 1;
  color: var(--muted);
  line-height: 1.85;
  font-size: 0.9rem;
}
.service-card ul {
  position: relative;
  z-index: 1;
  display: grid;
  gap: 7px;
  margin-top: 20px;
}
.service-card li {
  display: flex;
  align-items: center;
  gap: 8px;
  color: var(--soft);
  font-size: 0.82rem;
  font-weight: 800;
}
.service-card li::before {
  content: "";
  flex: 0 0 6px;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--green);
}

/* ---------- Why Us ---------- */
.why-panel {
  position: sticky;
  top: 100px;
  padding: clamp(18px, 3vw, 28px);
  border: 1px solid var(--line);
  border-radius: var(--radius-xl);
  background: var(--card-bg);
  box-shadow: var(--shadow-1);
  backdrop-filter: blur(var(--blur-md));
}
.why-list {
  display: grid;
  gap: 14px;
}
.why-item {
  position: relative;
  display: grid;
  grid-template-columns: 56px 1fr;
  gap: 16px;
  padding: 22px;
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  background: var(--card-bg);
  box-shadow: var(--shadow-1);
  backdrop-filter: blur(var(--blur-soft));
  overflow: hidden;
  transition:
    transform 0.25s var(--ease-standard),
    border-color 0.25s var(--ease-standard),
    box-shadow 0.25s var(--ease-standard);
}
.why-item:hover {
  transform: translateY(-5px);
  border-color: rgba(45, 212, 255, 0.28);
  box-shadow: var(--shadow-2), var(--overlay-glow);
}
.why-item > span {
  position: relative;
  z-index: 1;
  display: grid;
  place-items: center;
  width: 50px;
  height: 50px;
  border-radius: 18px;
  border: 1px solid rgba(45, 212, 255, 0.22);
  background: rgba(45, 212, 255, 0.06);
  color: var(--cyan);
  font-size: 0.9rem;
  font-weight: 1000;
}
.why-item h3 {
  position: relative;
  z-index: 1;
  margin-bottom: 7px;
  color: var(--text-heading);
  font-size: 1.08rem;
  font-weight: 1000;
}
.why-item p {
  position: relative;
  z-index: 1;
  color: var(--muted);
  line-height: 1.85;
  font-size: 0.9rem;
}

/* ---------- Testimonials ---------- */
.work-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 16px;
}
.project-card {
  position: relative;
  min-height: 420px;
  overflow: hidden;
  border: 1px solid var(--line);
  border-radius: var(--radius-xl);
  background: var(--card-bg);
  box-shadow: var(--shadow-1);
  backdrop-filter: blur(var(--blur-soft));
  transition:
    transform 0.3s var(--ease-standard),
    border-color 0.3s var(--ease-standard),
    box-shadow 0.3s var(--ease-standard);
}
.project-card:hover {
  transform: translateY(-8px);
  border-color: rgba(45, 212, 255, 0.32);
  box-shadow: var(--shadow-2), var(--overlay-glow);
}
.project-visual {
  position: relative;
  height: 150px;
  overflow: hidden;
  border-bottom: 1px solid var(--line);
  background:
    radial-gradient(
      circle at 25% 20%,
      rgba(45, 212, 255, 0.14),
      transparent 30%
    ),
    radial-gradient(
      circle at 75% 25%,
      rgba(139, 92, 246, 0.14),
      transparent 32%
    ),
    linear-gradient(135deg, rgba(45, 212, 255, 0.04), rgba(139, 92, 246, 0.04));
}
.project-glow {
  position: absolute;
  inset: auto 50% 20px auto;
  width: 130px;
  height: 130px;
  border-radius: 50%;
  background: var(--cyan);
  filter: blur(40px);
  opacity: 0.24;
  transform: translateX(50%);
}
.project-window {
  position: absolute;
  inset-inline: 22px;
  bottom: 22px;
  height: 60px;
  border: 1px solid rgba(148, 163, 184, 0.2);
  border-radius: 20px;
  background: var(--terminal-bg);
  box-shadow: 0 16px 48px rgba(0, 0, 0, 0.18);
  backdrop-filter: blur(14px);
}
.project-window span {
  position: absolute;
  inset-inline: 14px;
  height: 6px;
  border-radius: 999px;
  background: rgba(128, 128, 128, 0.1);
}
.project-window span:nth-child(1) {
  top: 14px;
  width: 38%;
}
.project-window span:nth-child(2) {
  top: 28px;
  width: 65%;
}
.project-window span:nth-child(3) {
  top: 42px;
  width: 48%;
}
.project-content {
  padding: 22px;
}
.testimonial-quote {
  margin: 0;
}
.testimonial-quote h3 {
  margin-bottom: 10px;
  color: var(--text-heading);
  font-size: 1.1rem;
  font-weight: 1000;
}
.testimonial-quote p {
  color: var(--muted);
  line-height: 1.85;
  font-size: 0.88rem;
}
.testimonial-quote footer.project-meta {
  margin-top: 16px;
  padding: 0;
}
.project-meta {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 8px 12px;
}
.project-meta cite {
  font-style: normal;
  color: var(--text-heading);
  font-size: 0.8rem;
  font-weight: 900;
}
.project-meta span {
  color: var(--soft);
  font-size: 0.78rem;
  font-weight: 700;
}
.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 16px;
}
.project-tags span {
  display: inline-flex;
  align-items: center;
  min-height: 28px;
  padding-inline: 9px;
  border-radius: var(--radius-pill);
  background: rgba(45, 212, 255, 0.08);
  color: var(--cyan);
  font-size: 0.72rem;
  font-weight: 900;
}

/* ---------- Process & Search Features ---------- */
.process-line {
  position: relative;
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 16px;
}
.process-line--five {
  grid-template-columns: repeat(3, minmax(0, 1fr));
}
.process-card {
  position: relative;
  z-index: 1;
  min-height: 220px;
  padding: 22px;
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  background: var(--card-bg);
  box-shadow: var(--shadow-1);
  backdrop-filter: blur(var(--blur-soft));
  transition:
    transform 0.25s var(--ease-standard),
    border-color 0.25s var(--ease-standard),
    box-shadow 0.25s var(--ease-standard);
}
.process-card:hover {
  transform: translateY(-6px);
  border-color: rgba(45, 212, 255, 0.28);
  box-shadow: var(--shadow-2), var(--overlay-glow);
}
.step-number {
  display: grid;
  place-items: center;
  width: 54px;
  height: 54px;
  margin-bottom: 26px;
  border-radius: 20px;
  border: 1px solid rgba(45, 212, 255, 0.22);
  background: rgba(45, 212, 255, 0.06);
  color: var(--cyan);
  font-size: 0.9rem;
  font-weight: 1000;
}
.process-card h3 {
  margin-bottom: 9px;
  color: var(--text-heading);
  font-size: 1.08rem;
  font-weight: 1000;
}
.process-card p {
  color: var(--muted);
  line-height: 1.85;
  font-size: 0.88rem;
}

/* ---------- Contact ---------- */
.contact-section {
  padding-bottom: clamp(80px, 10vw, 120px);
}
.contact-card {
  position: relative;
  display: grid;
  grid-template-columns: minmax(0, 0.92fr) minmax(320px, 1.08fr);
  gap: 32px;
  overflow: hidden;
  padding: clamp(18px, 3vw, 30px);
  border: 1px solid rgba(45, 212, 255, 0.18);
  border-radius: 38px;
  background: var(--card-bg);
  box-shadow: var(--shadow-2), var(--overlay-glow);
  backdrop-filter: blur(var(--blur-md));
}
.contact-copy {
  align-self: center;
}
.contact-info {
  display: grid;
  gap: 10px;
  margin-top: 24px;
  font-style: normal;
}
.contact-info a {
  width: fit-content;
  color: var(--muted);
  font-weight: 900;
  transition:
    color 0.2s var(--ease-standard),
    transform 0.2s var(--ease-standard);
}
.contact-info a:hover {
  color: var(--cyan);
}
.contact-address {
  color: var(--muted);
  font-size: var(--text-sm);
  margin-top: 6px !important;
}

.contact-form {
  display: grid;
  gap: 13px;
  padding: 20px;
  border: 1px solid var(--line);
  border-radius: 26px;
  background: var(--surface-3);
}
.contact-form label {
  display: grid;
  gap: 6px;
  color: var(--muted);
  font-size: 0.88rem;
  font-weight: 900;
}
.hp-field {
  position: absolute !important;
  width: 1px;
  height: 1px;
  overflow: hidden;
  opacity: 0;
  pointer-events: none;
}

input,
textarea {
  width: 100%;
  border: 1px solid var(--line);
  outline: none;
  border-radius: 16px;
  background: var(--input-bg);
  color: var(--text);
  transition:
    border-color 0.22s var(--ease-standard),
    box-shadow 0.22s var(--ease-standard),
    background 0.22s var(--ease-standard);
}
input {
  min-height: 50px;
  padding-inline: 14px;
}
textarea {
  min-height: 140px;
  resize: vertical;
  padding: 12px 14px;
  line-height: 1.7;
}
input::placeholder,
textarea::placeholder {
  color: rgba(128, 128, 128, 0.5);
}
input:focus,
textarea:focus {
  border-color: rgba(45, 212, 255, 0.5);
  background: var(--input-focus-bg);
  box-shadow: 0 0 0 3px rgba(45, 212, 255, 0.08);
}
input[aria-invalid="true"],
textarea[aria-invalid="true"] {
  border-color: var(--danger);
}
.field-error {
  color: var(--danger);
  font-size: 0.78rem;
  font-weight: 800;
}
.form-message {
  color: var(--green);
  font-size: 0.9rem;
  font-weight: 900;
  line-height: 1.7;
}

/* ---------- Footer ---------- */
.footer {
  position: relative;
  padding-block: 42px;
  border-top: 1px solid var(--line);
}
.footer-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(220px, 0.8fr) minmax(
      200px,
      0.7fr
    );
  gap: 24px;
  align-items: center;
}
.footer-brand {
  margin-bottom: 14px;
}
.footer p {
  max-width: 480px;
  color: var(--muted);
  line-height: 1.85;
  font-size: 0.9rem;
  font-weight: 500;
}
.footer-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px 14px;
}
.footer-links a {
  color: var(--muted);
  font-size: 0.88rem;
  font-weight: 900;
  transition: color 0.2s var(--ease-standard);
}
.footer-links a:hover {
  color: var(--cyan);
}

/* Back to top */
.back-to-top {
  position: fixed;
  bottom: max(20px, env(safe-area-inset-bottom));
  inset-inline-end: 20px;
  z-index: 900;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: 1px solid var(--line-2);
  background: var(--surface);
  color: var(--text-heading);
  font-size: 1.1rem;
  box-shadow: var(--shadow-2);
  backdrop-filter: blur(var(--blur-soft));
  transition:
    transform 0.2s var(--ease-standard),
    border-color 0.2s var(--ease-standard);
}
.back-to-top:hover {
  transform: translateY(-3px);
  border-color: rgba(45, 212, 255, 0.4);
}

/* Vue transitions */
.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.2s ease,
    transform 0.2s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(8px);
}

/* Reveal animations */
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition:
    opacity 0.7s var(--ease-smooth),
    transform 0.7s var(--ease-smooth);
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

:where(a, button, input, textarea, select, [tabindex]):focus-visible {
  outline: 2px solid rgba(45, 212, 255, 0.55);
  outline-offset: 3px;
}

/* ---------- Keyframes ---------- */
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(53, 242, 169, 0.5);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(53, 242, 169, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(53, 242, 169, 0);
  }
}
@keyframes blink {
  0%,
  45% {
    opacity: 1;
  }
  46%,
  100% {
    opacity: 0;
  }
}
@keyframes float {
  0%,
  100% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, -14px, 0);
  }
}

/* =========================================================
   HAMBURGER MENU (Mobile) – SOLID, NO FLASH ON RESIZE
   ========================================================= */
.hamburger {
  display: none;
  position: relative;
  width: 44px;
  height: 44px;
  border: 1px solid var(--line);
  border-radius: 15px;
  background: var(--surface-3);
  flex-shrink: 0;
}
.hamburger span {
  position: absolute;
  inset-inline-start: 11px;
  width: 20px;
  height: 2px;
  border-radius: 999px;
  background: var(--text);
  transition:
    transform 0.25s var(--ease-standard),
    top 0.25s var(--ease-standard),
    opacity 0.25s var(--ease-standard);
}
.hamburger span:first-child {
  top: 16px;
}
.hamburger span:last-child {
  top: 26px;
}

.menu-open .hamburger span:first-child {
  top: 21px;
  transform: rotate(45deg);
}
.menu-open .hamburger span:last-child {
  top: 21px;
  transform: rotate(-45deg);
}

.nav-backdrop {
  position: fixed;
  inset: 0;
  z-index: 998;
  background: rgba(0, 0, 0, 0.6);
  border: 0;
  cursor: pointer;
  animation: fadeIn 0.2s ease;
}
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Mobile menu panel – transition: none when closed to prevent flash */
@media (max-width: 1200px) {
  .hamburger {
    display: block;
    justify-self: end;
  }

  .nav-menu {
    position: fixed;
    top: calc(var(--header-h) + 6px);
    left: 12px;
    right: 12px;
    z-index: 999;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
    padding: 18px;
    border-radius: 24px;
    background: #012438;
    box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
    max-height: calc(100svh - var(--header-h) - 20px);
    overflow-y: auto;

    /* Closed state: no transition, instantly hidden */
    opacity: 0;
    visibility: hidden;
    transform: translateY(-12px) scale(0.98);
    transition: none;
  }

  .menu-open .nav-menu {
    opacity: 1;
    visibility: visible;
    transform: translateY(0) scale(1);
    /* Open animation only when menu-open class is added */
    transition:
      opacity 0.3s ease,
      transform 0.3s ease,
      visibility 0.3s ease;
  }

  .nav-menu a {
    min-height: 48px;
    padding: 12px 12px;
    border-radius: 16px;
    background: rgba(255, 255, 255, 0.04);
    border: 1px solid rgba(255, 255, 255, 0.08);
    color: #eaf2ff;
    font-size: 0.88rem;
    font-weight: 700;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    white-space: normal;
    word-break: break-word;
    transition: all 0.25s ease;
  }

  .nav-menu a:hover,
  .nav-menu a:focus-visible {
    background: rgba(45, 212, 255, 0.15);
    border-color: rgba(45, 212, 255, 0.35);
    color: var(--cyan);
    transform: translateY(-1px);
  }

  .nav-menu a.is-active,
  .nav-menu a[aria-current="page"] {
    background: rgba(45, 212, 255, 0.2);
    border-color: rgba(45, 212, 255, 0.5);
    color: var(--cyan);
    font-weight: 800;
  }
}

/* ensure two columns on all small screens */
@media (max-width: 720px) {
  .nav-menu {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 480px) {
  .nav-menu {
    grid-template-columns: repeat(2, 1fr) !important;
  }
}

/* ---------- Global Responsive ---------- */
@media (max-width: 1360px) {
  .nav {
    gap: 12px;
  }
  .nav-menu a {
    padding-inline: 11px;
    font-size: 0.81rem;
  }
  .btn-small {
    padding-inline: 14px;
    font-size: 0.78rem;
  }
}
@media (max-width: 1240px) {
  .nav-menu a {
    padding-inline: 9px;
    font-size: 0.78rem;
  }
  .btn-small {
    min-height: 38px;
    padding-inline: 12px;
    font-size: 0.74rem;
  }
  .nav-actions {
    gap: 6px;
  }
  .icon-btn {
    width: 40px;
    height: 40px;
  }
  .hero-grid,
  .split-grid,
  .why-grid,
  .contact-card {
    gap: 32px;
  }
  .services-grid,
  .work-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .services-grid--three {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .process-line,
  .process-line--five {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .process-line::before {
    display: none;
  }
}
@media (max-width: 1200px) {
  .nav {
    grid-template-columns: auto 1fr auto;
    gap: 10px;
  }
  .nav-actions .btn-small {
    display: none;
  }
}
@media (max-width: 1024px) {
  .split-grid,
  .why-grid,
  .contact-card {
    grid-template-columns: 1fr;
  }
  .why-panel {
    position: relative;
    top: auto;
  }
  .hero-visual {
    min-height: 500px;
  }
}
@media (max-width: 940px) {
  :root {
    --header-h: 72px;
    --container-pad: 14px;
  }
  .nav {
    gap: 8px;
  }
  .icon-btn,
  .hamburger {
    width: 40px;
    height: 40px;
  }
  .hero {
    min-height: auto;
    padding-top: calc(var(--header-h) + 42px);
  }
  .hero-grid {
    grid-template-columns: 1fr;
  }
  .footer-grid {
    grid-template-columns: 1fr;
    text-align: center;
  }
  .footer .brand {
    margin-inline: auto;
  }
  .footer p {
    margin-inline: auto;
  }
  .footer-end {
    justify-items: center;
  }
}
@media (max-width: 720px) {
  .brand-copy small {
    display: none;
  }
  .hero-actions {
    display: grid;
  }
  .hero-actions .btn {
    width: 100%;
  }
  .hero-stats {
    grid-template-columns: 1fr;
  }
  .hero-visual {
    min-height: auto;
    padding: 20px 0 82px;
  }
  .orbit-one {
    width: min(100%, 340px);
  }
  .orbit-two {
    width: min(72%, 250px);
  }
  .terminal-card {
    border-radius: 24px;
  }
  .terminal-body {
    padding: 16px;
  }
  .terminal-body p {
    font-size: 0.72rem;
  }
  .insight-card {
    width: 150px;
    padding: 10px;
  }
  .insight-top {
    top: auto;
    bottom: 14px;
    inset-inline-end: 6px;
  }
  .insight-bottom {
    bottom: -48px;
    inset-inline-start: 6px;
  }
  .center-heading {
    text-align: start;
  }
  .center-heading .section-label {
    margin-inline: 0;
  }
  .services-grid,
  .services-grid--three,
  .work-grid,
  .process-line,
  .process-line--five {
    grid-template-columns: 1fr;
  }
  .project-card,
  .service-card,
  .process-card {
    min-height: auto;
  }
  .contact-form {
    padding: 14px;
  }
  .why-item {
    grid-template-columns: 1fr;
  }
  .back-to-top {
    width: 44px;
    height: 44px;
    inset-inline-end: 14px;
    bottom: max(14px, env(safe-area-inset-bottom));
  }
}
@media (max-width: 560px) {
  .nav-actions .lang-switch {
    display: none;
  }
  .brand-copy strong {
    max-width: 120px;
  }
}
@media (max-width: 480px) {
  .section {
    padding-block: 56px;
  }
  .hero {
    padding-top: calc(var(--header-h) + 34px);
  }
  .brand-symbol {
    width: 40px;
    height: 40px;
    border-radius: 14px;
  }
  .about-copy,
  .why-panel,
  .contact-card {
    padding: 16px;
  }
  .about-point {
    flex-direction: column;
  }
  .project-visual {
    height: 130px;
  }
  .footer-links {
    display: grid;
  }
}
@media (max-width: 420px) {
  .brand-symbol {
    width: 38px;
    height: 38px;
  }
  .brand-copy strong {
    max-width: 96px;
    font-size: 0.76rem;
  }
  .icon-btn,
  .hamburger {
    width: 36px;
    height: 36px;
  }
  .hamburger span {
    inset-inline-start: 9px;
    width: 18px;
  }
  .hamburger span:first-child {
    top: 13px;
  }
  .hamburger span:last-child {
    top: 21px;
  }
  .menu-open .hamburger span:first-child,
  .menu-open .hamburger span:last-child {
    top: 17px;
  }
  body.ltr .brand-copy strong {
    font-size: 0.76rem;
  }
  body.ltr .nav-menu a {
    font-size: 0.78rem;
  }
}
@media (max-width: 360px) {
  .hero h1 {
    letter-spacing: -0.02em;
  }
  .status-pill {
    font-size: 0.72rem;
  }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto !important;
  }
  *,
  *::before,
  *::after {
    animation-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.001ms !important;
  }
  .reveal {
    opacity: 1;
    transform: none;
  }
}

/* Print */
@media print {
  .header,
  .nav-backdrop,
  .back-to-top,
  .hero-visual,
  .aurora-blue,
  .noise-layer {
    display: none !important;
  }
  body {
    background: #fff;
    color: #000;
  }
}
</style>
