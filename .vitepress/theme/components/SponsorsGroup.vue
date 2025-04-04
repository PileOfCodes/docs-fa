<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { SponsorData, data, base, load } from './sponsors'

type Placement = 'aside' | 'page' | 'landing'

const props = withDefaults(
  defineProps<{
    tier: keyof SponsorData
    placement?: Placement
  }>(),
  {
    placement: 'aside'
  }
)

const container = ref<HTMLElement>()
const visible = ref(false)

onMounted(async () => {
  // only render when entering view
  const observer = new IntersectionObserver(
    (entries) => {
      if (entries[0].isIntersecting) {
        visible.value = true
        observer.disconnect()
      }
    },
    { rootMargin: '0px 0px 300px 0px' }
  )
  observer.observe(container.value!)
  onUnmounted(() => observer.disconnect())

  // load data
  await load()
})

// fathom events
const eventMap: Record<Placement, string> = {
  aside: '4QUPDDRU',
  landing: '58FLAR2Z',
  page: 'ZXLO3IUT'
}

function track(interest?: boolean) {
  fathom.trackGoal(interest ? `Y2BVYNT2` : eventMap[props.placement], 0)
}
</script>

<template>
  <div
    ref="container"
    dir="rtl"
    class="sponsor-container"
    :class="[tier.startsWith('plat') ? 'platinum' : tier, placement]"
  >
    <template v-if="data && visible">
      <a
        v-for="{ url, img, name } of data[tier]"
        class="sponsor-item"
        :href="url"
        target="_blank"
        rel="sponsored noopener"
        @click="track()"
      >
        <picture v-if="img.endsWith('png')">
          <source
            type="image/avif"
            :srcset="`${base}/images/${img.replace(/\.png$/, '.avif')}`"
          />
          <img :src="`${base}/images/${img}`" :alt="name" />
        </picture>
        <img v-else :src="`${base}/images/${img}`" :alt="name" />
      </a>
    </template>
    <a
      v-if="placement !== 'page' && tier !== 'special'"
      href="/sponsor/"
      class="sponsor-item action"
      @click="track(true)"
      >حامی شوید</a
    >
  </div>
</template>

<style scoped>
.sponsor-container {
  --max-width: 100%;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(var(--max-width), 1fr));
  column-gap: 4px;
}

.sponsor-container.platinum {
  --max-width: 240px;
}
.sponsor-container.gold {
  --max-width: 180px;
}
.sponsor-container.silver {
  --max-width: 140px;
}

.sponsor-item {
  margin: 2px 0;
  background-color: var(--vt-c-white-soft);
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-radius: 2px;
  transition: background-color 0.2s ease;
  height: calc(var(--max-width) / 2 - 6px);
}
.sponsor-item.action {
  font-size: 11px;
  color: var(--vt-c-text-3);
}
.sponsor-item img {
  max-width: calc(var(--max-width) - 30px);
  max-height: calc(var(--max-width) / 2 - 20px);
}
.special .sponsor-item {
  height: 160px;
}
.special .sponsor-item img {
  max-width: 300px;
  max-height: 150px;
}

/* dark mode */
.dark .aside .sponsor-item,
.dark .landing .sponsor-item {
  background-color: var(--vt-c-bg-soft);
}
.aside .sponsor-item img,
.landing .sponsor-item img {
  transition: filter 0.2s ease;
}
.dark .aside .sponsor-item img,
.dark .landing .sponsor-item img {
  filter: grayscale(1) invert(1);
}
.dark .aside .sponsor-item:hover,
.dark .landing .sponsor-item:hover {
  color: var(--vt-c-indigo);
  background-color: var(--vt-c-white-mute);
}
.dark .sponsor-item:hover img {
  filter: none;
}

/* aside mode (on content pages) */
.sponsor-container.platinum.aside {
  --max-width: 110px;
  column-gap: 1px;
}
.aside .sponsor-item {
  margin: 1px 0;
}
.aside .special .sponsor-item {
  width: 100%;
  height: 60px;
}
.aside .special .sponsor-item img {
  width: 120px;
}
.aside .platinum .sponsor-item {
  width: 111px;
  height: 50px;
}
.aside .platinum .sponsor-item img {
  max-width: 88px;
}

/* narrow, aside will be hidden under this state so it's mutually exclusive */
@media (max-width: 720px) {
  .sponsor-container.platinum {
    --max-width: 180px;
  }
  .sponsor-container.gold {
    --max-width: 140px;
  }
  .sponsor-container.silver {
    --max-width: 120px;
  }
}

@media (max-width: 480px) {
  .sponsor-container.platinum {
    --max-width: 150px;
  }
  .sponsor-container.gold {
    --max-width: 120px;
  }
  .sponsor-container.silver {
    --max-width: 100px;
  }
}
</style>
