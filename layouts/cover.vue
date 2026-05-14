<script setup lang="ts">
import { computed } from 'vue'
import { handleBackground } from '../layoutHelper'

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

const style = computed(() => handleBackground(props.background, true))
const metaLines = computed(() => Array.isArray(props.coverMeta)
  ? props.coverMeta
  : props.coverMeta
    ? [props.coverMeta]
    : [])
const useStructuredCover = computed(() =>
  !!props.coverDate
  || !!props.coverTitle
  || metaLines.value.length > 0
  || !!props.coverBodyTitle)
</script>

<template>
  <div class="slidev-layout cover" :style="style">
    <div v-if="useStructuredCover" class="my-auto w-full">
      <div class="cover-shell">
        <div v-if="props.coverDate" class="cover-shell__date">
          {{ props.coverDate }}
        </div>

        <h1 v-if="props.coverTitle" class="cover-shell__title">
          {{ props.coverTitle }}
        </h1>

        <div v-if="metaLines.length" class="cover-shell__meta">
          <p v-for="line in metaLines" :key="line">
            {{ line }}
          </p>
        </div>

        <div class="cover-shell__body">
          <div v-if="props.coverBodyTitle" class="cover-shell__body-title">
            {{ props.coverBodyTitle }}
          </div>
          <slot />
        </div>
      </div>
    </div>
    <div v-else class="my-auto w-full">
      <slot />
    </div>
  </div>
</template>