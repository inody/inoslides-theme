<script setup lang="ts">
import { computed, inject } from 'vue'

const slidevContext = inject<Record<string, any> | undefined>('$$slidev-context', undefined)
const frontmatter = inject<Record<string, string | number> | undefined>('$$slidev-fontmatter', undefined)

const displayTotal = computed(() => slidevContext?.configs?.pageTotal ?? frontmatter?.pageTotal)
</script>

<template>
  <footer class="absolute bottom-0 left-226 right-0 p-2">
    {{ $slidev.nav.currentPage }} / {{ displayTotal ?? $slidev.nav.total }}
  </footer>
</template>