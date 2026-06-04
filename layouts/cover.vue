<script setup lang="ts">
import type { CSSProperties } from 'vue'
import { computed } from 'vue'
import { resolveAssetUrl } from '../layoutHelper'

const props = defineProps({
  background: {
    default: '',
  },
  coverDate: {
    type: String,
    default: '',
  },
  coverTitle: {
    type: String,
    default: '',
  },
  coverMeta: {
    type: [Array, String],
    default: () => [],
  },
  coverBodyTitle: {
    type: String,
    default: '',
  },
})

const isColorBackground = computed(() =>
  props.background && ['#', 'rgb', 'hsl'].some(v => props.background.indexOf(v) === 0))
const backgroundUrl = computed(() =>
  props.background && !isColorBackground.value ? resolveAssetUrl(props.background) : '')
const style = computed((): CSSProperties => ({
  background: isColorBackground.value ? props.background : '#fff',
  color: '#0f172a',
  position: 'relative',
  overflow: 'hidden',
}))
const metaLines = computed(() => Array.isArray(props.coverMeta)
  ? (props.coverMeta as string[])
  : props.coverMeta
    ? [props.coverMeta as string]
    : [])
const useStructuredCover = computed(() =>
  !!props.coverDate
  || !!props.coverTitle
  || metaLines.value.length > 0
  || !!props.coverBodyTitle)
</script>

<template>
  <div class="slidev-layout cover" :style="style">
    <div
      v-if="backgroundUrl"
      aria-hidden="true"
      style="position:absolute; inset:-28px; z-index:0; background-repeat:no-repeat; background-position:center; background-size:cover; filter:blur(8px) saturate(0.95); opacity:0.60; transform:scale(1.04);"
      :style="{ backgroundImage: `url(${backgroundUrl})` }"
    />
    <div
      v-if="backgroundUrl"
      aria-hidden="true"
      style="position:absolute; inset:0; z-index:0; background:rgba(255,255,255,0.42);"
    />
    <div v-if="useStructuredCover" class="my-auto w-full" style="position:relative; z-index:1;">
      <div class="cover-shell" style="gap: 0.6rem;">
        <div v-if="props.coverDate" class="cover-shell__date">
          {{ props.coverDate }}
        </div>

        <h1 v-if="props.coverTitle" class="cover-shell__title">
          {{ props.coverTitle }}
        </h1>

        <div v-if="metaLines.length" class="cover-shell__meta">
          <p v-for="(line, i) in metaLines" :key="i">
            {{ line }}
          </p>
        </div>

        <div class="cover-shell__body" style="border-top:0; padding-top:0;">
          <div v-if="props.coverBodyTitle" class="cover-shell__body-title">
            {{ props.coverBodyTitle }}
          </div>
          <slot />
        </div>
      </div>
    </div>
    <div v-else class="my-auto w-full" style="position:relative; z-index:1;">
      <slot />
    </div>
  </div>
</template>
