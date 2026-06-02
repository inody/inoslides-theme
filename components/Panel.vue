<script setup lang="ts">
import { computed, useSlots } from 'vue'

const props = defineProps({
  title: {
    type: String,
    default: '',
  },
})

const slots = useSlots()
const hasHeader = computed(() => !!props.title || !!slots.title)
</script>

<template>
  <section class="theme-panel">
    <header v-if="hasHeader" class="theme-panel__header">
      <slot name="title">
        {{ props.title }}
      </slot>
    </header>
    <div class="theme-panel__body">
      <slot />
    </div>
  </section>
</template>

<style scoped>
.theme-panel {
  border: 1px solid rgb(148 163 184 / 0.35);
  border-radius: 16px;
  background: rgb(248 250 252 / 0.75);
  padding: 1rem 1.25rem;
  box-shadow: 0 8px 24px rgb(15 23 42 / 0.04);
}

.theme-panel__header {
  margin: 0 0 0.5rem;
  font-size: 0.95rem;
  font-weight: 700;
  line-height: 1.4;
  color: #334155;
}

.theme-panel__body {
  margin: 0;
  color: #0f172a;
}

.theme-panel__body :deep(p:first-child),
.theme-panel__body :deep(ul:first-child),
.theme-panel__body :deep(ol:first-child),
.theme-panel__body :deep(.katex-display:first-child) {
  margin-top: 0;
}

.theme-panel__body :deep(p:last-child),
.theme-panel__body :deep(ul:last-child),
.theme-panel__body :deep(ol:last-child),
.theme-panel__body :deep(.katex-display:last-child) {
  margin-bottom: 0;
}

.theme-panel__body :deep(p) {
  margin-bottom: 0.5em;
}

.theme-panel__body :deep(.katex-display) {
  margin: 0.8em 0;
}
</style>