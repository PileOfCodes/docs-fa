<script setup lang="ts">
import { onMounted } from 'vue'
import SiteMap from './SiteMap.vue'
// import NewsLetter from './NewsLetter.vue'
import { load, data, base } from './sponsors'
import SponsorsGroup from './SponsorsGroup.vue'
import VueMasteryModal from './VueMasteryModal.vue'

onMounted(async () => {
  await load()
})
</script>

<template>
  <section id="hero">
    <h1 class="tagline">
      فریمورک
      <span class="accent">پیش‌رونده</span>
      <br />جاوااسکریپت
    </h1>
    <p class="description">
      یک فریمورک <strong>آسان، بهینه و انعطاف‌پذیر</strong> برای ساخت روابط
      کاربری وب.
    </p>
    <div class="actions">
      <a class="get-started" href="/guide/introduction.html">
        <svg
          class="icon"
          xmlns="http://www.w3.org/2000/svg"
          width="10"
          height="10"
          viewBox="0 0 24 24"
        >
          <path
            d="M13.025 1l-2.847 2.828 6.176 6.176h-16.354v3.992h16.354l-6.176 6.176 2.847 2.828 10.975-11z"
          />
        </svg>
        شروع کنید
      </a>
      <a class="setup" href="/guide/quick-start.html">نصب</a>
      <VueMasteryModal />
    </div>
  </section>

  <section dir="rtl" id="special-sponsor">
    <span class="lead">حامی ویژه</span>
    <template v-if="data && data.special">
      <template v-for="{ url, img, name, description } of data.special">
        <a :href="url" target="_blank" rel="sponsored noopener">
          <picture v-if="img.endsWith('png')">
            <source
              type="image/avif"
              :srcset="`${base}/images/${img.replace(/\.png$/, '.avif')}`"
            />
            <img :src="`${base}/images/${img}`" :alt="name" />
          </picture>
          <img
            width="168"
            height="42"
            v-else
            :src="`${base}/images/${img}`"
            :alt="name"
          />
        </a>
        <span dir="ltr">{{ description }}</span>
      </template>
    </template>
  </section>

  <section id="highlights" class="vt-box-container">
    <div class="vt-box">
      <h2>آسان</h2>
      <p>
        براساس استانداردهای HTML، CSS و JavaScript ساخته شده و دارای
        مستندات قابل‌فهم با APIهای آسان برای استفاده است.
      </p>
    </div>
    <div class="vt-box">
      <h2>بهینه و با عملکرد بالا</h2>
      <p>
        سیستم رندر کاملا واکنشی و بهینه سازی شده توسط کامپایلر که به ندرت
        به بهینه سازی دستی نیاز دارد.
      </p>
    </div>
    <div class="vt-box">
      <h2>انعطاف‌پذیر</h2>
      <p dir="auto">
        یک اکوسیستم غنی و به تدریج درحال رشد که بین مقیاس یک کتابخانه و یک
        چارچوب با ویژگی‌های کامل قرار می‌گیرد.
      </p>
    </div>
  </section>

  <section id="sponsors">
    <h2>حامی‌های پلاتین</h2>
    <SponsorsGroup tier="platinum" placement="landing" />
    <h2>حامی‌های طلا</h2>
    <SponsorsGroup tier="gold" placement="landing" />
  </section>

  <SiteMap />
  <!-- <NewsLetter /> -->
</template>

<style scoped>
section {
  padding: 42px 32px;
}

#hero {
  padding: 96px 32px;
  text-align: center;
}

.tagline {
  font-size: 76px;
  line-height: 1.25;
  font-weight: 900;
  letter-spacing: -1.5px;
  max-width: 960px;
  margin: 0px auto;
}

html:not(.dark) .accent,
.dark .tagline {
  background: -webkit-linear-gradient(315deg, #42d392 25%, #647eff);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.description {
  max-width: 960px;
  line-height: 1.5;
  color: var(--vt-c-text-2);
  transition: color 0.5s;
  font-size: 22px;
  margin: 24px auto 40px;
}

.actions a {
  font-size: 16px;
  display: inline-block;
  background-color: var(--vt-c-bg-mute);
  padding: 8px 18px;
  font-weight: 500;
  border-radius: 8px;
  transition: background-color 0.5s, color 0.5s;
}

.actions .get-started {
  margin-right: 18px;
}

.actions .icon {
  display: inline;
  position: relative;
  top: -1px;
  margin-left: 2px;
  fill: currentColor;
  transform: rotate(180deg);
  transition: transform 0.2s;
}

.actions .get-started:hover {
  transition-duration: 0.2s;
}

.actions .get-started:hover .icon {
  transform: rotate(180deg) translateX(2px);
}

.actions .get-started,
.actions .setup {
  color: var(--vt-c-text-code);
}

.actions .get-started:hover,
.actions .setup:hover {
  background-color: var(--vt-c-gray-light-4);
  transition-duration: 0.2s;
}

.dark .actions .get-started:hover,
.dark .actions .setup:hover {
  background-color: var(--vt-c-gray-dark-3);
}

#special-sponsor {
  border-top: 1px solid var(--vt-c-divider-light);
  border-bottom: 1px solid var(--vt-c-divider-light);
  padding: 12px 24px;
  display: flex;
  align-items: center;
}

#special-sponsor span {
  color: var(--vt-c-text-2);
  font-weight: 500;
  font-size: 13px;
  vertical-align: middle;
  text-align: end;
  flex: 1;
}

#special-sponsor span:first-child {
  text-align: left;
}

#special-sponsor a {
  display: flex;
  justify-content: center;
  padding: 0 24px;
}

#special-sponsor img {
  height: 42px;
  margin: -6px 0;
}

.dark #special-sponsor img {
  filter: grayscale(1) invert(1);
}

#highlights {
  max-width: 960px;
  margin: 0px auto;
  color: var(--vt-c-text-2);
}

#highlights h2 {
  font-weight: 600;
  font-size: 20px;
  letter-spacing: -0.4px;
  color: var(--vt-c-text-1);
  transition: color 0.5s;
  margin-bottom: 0.75em;
}

#highlights p {
  font-weight: 400;
  font-size: 15px;
}

#highlights .vt-box {
  background-color: transparent;
}

#sponsors {
  max-width: 900px;
  margin: 0px auto;
}

#sponsors h2 {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 1em;
}

#sponsors .sponsor-container {
  margin-bottom: 3em;
}

@media (max-width: 960px) {
  .tagline {
    font-size: 64px;
    letter-spacing: -0.5px;
  }
  .description {
    font-size: 18px;
    margin-bottom: 48px;
  }
}

@media (max-width: 768px) {
  .tagline {
    font-size: 48px;
    letter-spacing: -0.5px;
  }
}

@media (max-width: 576px) {
  #hero {
    padding: 56px 32px;
  }
  .description {
    font-size: 16px;
    margin: 18px 0 30px;
  }
  #special-sponsor {
    flex-direction: column;
  }
  #special-sponsor img {
    height: 36px;
    margin: 8px 0;
  }
  #special-sponsor span {
    text-align: center !important;
  }
  #highlights h3 {
    margin-bottom: 0.6em;
  }
  #highlights .vt-box {
    padding: 20px 36px;
  }
  .actions a {
    margin: 18px 0;
  }
}

@media (max-width: 370px) {
  .tagline {
    font-size: 36px;
  }
}
</style>
