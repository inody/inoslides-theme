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
  <section class="theme-pseudocode">
    <header v-if="hasHeader" class="theme-pseudocode__header">
      <div class="theme-pseudocode__title">
        <slot name="title">
          {{ props.title }}
        </slot>
      </div>
    </header>

    <div class="theme-pseudocode__body">
      <slot />
    </div>
  </section>
</template>

<style scoped>
.theme-pseudocode {
  border: 1px solid rgb(148 163 184 / 0.4);
  border-radius: 18px;
  background: rgb(248 250 252 / 0.82);
  padding: 1rem 1.25rem 1.1rem;
  box-shadow: 0 8px 24px rgb(15 23 42 / 0.04);
}

.theme-pseudocode__header {
  margin: 0 0 0.85rem;
}

.theme-pseudocode__title {
  min-width: 0;
  color: #0f172a;
  font-size: 1.02rem;
  font-weight: 700;
  line-height: 1.4;
}

.theme-pseudocode__body {
  color: #0f172a;
  font-size: 1rem;
  line-height: 1.55;
}

.theme-pseudocode__body :deep(ol:first-child),
.theme-pseudocode__body :deep(ul:first-child),
.theme-pseudocode__body :deep(p:first-child),
.theme-pseudocode__body :deep(.katex-display:first-child) {
  margin-top: 0;
}

.theme-pseudocode__body :deep(ol:last-child),
.theme-pseudocode__body :deep(ul:last-child),
.theme-pseudocode__body :deep(p:last-child),
.theme-pseudocode__body :deep(.katex-display:last-child) {
  margin-bottom: 0;
}

.theme-pseudocode__body :deep(ol) {
  margin: 0;
  padding-left: 1.4rem;
}

.theme-pseudocode__body :deep(ol > li) {
  margin: 0.8rem 0 0;
}

.theme-pseudocode__body :deep(ol > li:first-child) {
  margin-top: 0;
}

.theme-pseudocode__body :deep(ol > li > p:first-child) {
  margin-top: 0;
}

.theme-pseudocode__body :deep(ol > li > p) {
  margin-bottom: 0.4rem;
}

.theme-pseudocode__body :deep(ol > li > p > strong:first-child) {
  color: #0f172a;
  font-size: 1.03em;
}

.theme-pseudocode__body :deep(ul) {
  margin: 0.35rem 0 0;
  padding-left: 1.15rem;
}

.theme-pseudocode__body :deep(ul > li) {
  margin: 0.18rem 0;
  padding-left: 0.1rem;
}

.theme-pseudocode__body :deep(ul > li::marker) {
  color: #64748b;
}

.theme-pseudocode__body :deep(.katex-display) {
  margin: 0.65em 0;
}
</style>