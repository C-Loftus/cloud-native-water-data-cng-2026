<!--
  Wave motif rendered behind every slide, plus the CGS logo lockup pinned to the
  bottom-right corner of every slide.
  Three static, phase-offset SVG bands with gradient fills to suggest layered water.
-->
<template>
  <div class="water-waves" aria-hidden="true">
    <svg class="wave-stack" viewBox="0 0 1440 180" preserveAspectRatio="none">
      <defs>
        <linearGradient id="wave-back-fill" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0%" stop-color="#2c84b9" stop-opacity="0.34" />
          <stop offset="55%" stop-color="#46ab9d" stop-opacity="0.30" />
          <stop offset="100%" stop-color="#2c84b9" stop-opacity="0.38" />
        </linearGradient>
        <linearGradient id="wave-mid-fill" x1="1" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#46ab9d" stop-opacity="0.34" />
          <stop offset="60%" stop-color="#86c1e2" stop-opacity="0.24" />
          <stop offset="100%" stop-color="#46ab9d" stop-opacity="0.30" />
        </linearGradient>
        <linearGradient id="wave-front-fill" x1="0" y1="0" x2="1" y2="0">
          <stop offset="0%" stop-color="#86c1e2" stop-opacity="0.26" />
          <stop offset="50%" stop-color="#c9dee8" stop-opacity="0.32" />
          <stop offset="100%" stop-color="#86c1e2" stop-opacity="0.26" />
        </linearGradient>
      </defs>

      <!-- Back band: widest, slowest curve, sits highest -->
      <path :d="backPath" fill="url(#wave-back-fill)" />
      <!-- Mid band: phase-shifted against the back band so the crests interleave -->
      <path :d="midPath" fill="url(#wave-mid-fill)" />
      <!-- Front band: tightest curve, with a foam crest line along its top edge -->
      <path :d="frontPath" fill="url(#wave-front-fill)" />
      <path
        :d="frontCrest"
        fill="none"
        stroke="#c9dee8"
        stroke-opacity="0.42"
        stroke-width="1.6"
        vector-effect="non-scaling-stroke"
      />
    </svg>
  </div>

  <div v-if="showMark" class="cgs-mark">
    <img src="/cgs-logo-white.png" alt="The Center for Geospatial Solutions" />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

// A slide opts out of the corner lockup with `hideLogo: true` in its frontmatter.
// The live-demo slide does, so the mark never sits on top of the embedded viewer.
const { currentSlideRoute } = useNav()
const showMark = computed(
  () => currentSlideRoute.value?.meta?.slide?.frontmatter?.hideLogo !== true,
)

// Each band is one open crest line closed off along the bottom of the viewBox.
// The three lines use different amplitudes and phases so nothing lines up.
const backCrest = [
  'M0 100',
  'C 180 66, 300 134, 480 110',
  'S 780 58, 960 92',
  'S 1260 142, 1440 104',
].join(' ')

const midCrest = [
  'M0 130',
  'C 160 106, 320 156, 500 138',
  'S 820 96, 1000 128',
  'S 1300 162, 1440 134',
].join(' ')

const frontCrest = [
  'M0 152',
  'C 140 136, 280 168, 440 156',
  'S 720 130, 900 152',
  'S 1240 174, 1440 154',
].join(' ')

const close = (crest: string) => `${crest} L1440 180 L0 180 Z`

const backPath = close(backCrest)
const midPath = close(midCrest)
const frontPath = close(frontCrest)
</script>

<style scoped>
.water-waves {
  position: absolute;
  inset: auto 0 0 0;
  height: 46%;
  pointer-events: none;
  overflow: hidden;
  z-index: 0;
}

.wave-stack {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
}

/* Sits above the slide layout so it survives full-bleed slides (the iframe demo).
   The all-white lockup is the highest-contrast option against the dark blue stage. */
.cgs-mark {
  position: absolute;
  right: 1.15rem;
  bottom: 0.9rem;
  z-index: 20;
  pointer-events: none;
  line-height: 0;
}

.cgs-mark img {
  height: 2.1rem;
  width: auto;
}
</style>
