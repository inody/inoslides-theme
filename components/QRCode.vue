<script setup lang="ts">
import { renderSVG } from 'uqr'
import { computed } from 'vue'

const props = defineProps<{
  url: string
  size?: number
}>()

const svg = computed(() => renderSVG(props.url, { ecc: 'M' }))
const px = computed(() => `${props.size ?? 180}px`)
</script>

<template>
  <div :style="{ width: px, height: px }" v-html="svg" />
</template>
