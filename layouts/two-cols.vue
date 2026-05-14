<script setup lang="ts">
import { computed, useSlots } from 'vue'

const props = defineProps({
  class: {
    type: String,
  },
  leftSpan: {
    type: [Number, String],
    default: 5,
  },
  leftAlign: {
    type: String,
    default: 'center',
  },
  rightAlign: {
    type: String,
    default: 'center',
  },
  topAlign: {
    type: String,
    default: '',
  },
  bottomAlign: {
    type: String,
    default: 'center',
  },
})

const slots = useSlots()

const safeLeftSpan = computed(() => {
  const parsed = Number(props.leftSpan)
  if (Number.isNaN(parsed))
    return 5
  return Math.min(9, Math.max(1, Math.round(parsed)))
})

const safeRightSpan = computed(() => 10 - safeLeftSpan.value)

const leftStyle = computed(() => ({
  gridColumn: `span ${safeLeftSpan.value} / span ${safeLeftSpan.value}`,
}))

const rightStyle = computed(() => ({
  gridColumn: `span ${safeRightSpan.value} / span ${safeRightSpan.value}`,
}))

const hasLowerRow = computed(() => !!slots.lowerleft || !!slots.lowerright)

function textAlignClass(align?: string) {
  if (align === 'left')
    return 'text-left'
  if (align === 'right')
    return 'text-right'
  if (align === 'center')
    return 'text-center'
  return ''
}

const topAlignClass = computed(() => textAlignClass(props.topAlign))
const leftAlignClass = computed(() => textAlignClass(props.leftAlign))
const rightAlignClass = computed(() => textAlignClass(props.rightAlign))
const bottomAlignClass = computed(() => textAlignClass(props.bottomAlign))
</script>

<template>
  <div class="slidev-layout w-full h-full">
    <div :class="topAlignClass">
      <slot name="top" />
    </div>
    <div class="two-columns grid grid-cols-10 align-middle gap-10">
      <div :class="[props.class, leftAlignClass]" :style="leftStyle">
        <slot name="left" />
      </div>
      <div :class="[props.class, rightAlignClass]" :style="rightStyle">
        <slot name="right" />
      </div>
      <template v-if="hasLowerRow">
        <div :class="[props.class, leftAlignClass]" :style="leftStyle">
          <slot name="lowerleft" />
        </div>
        <div :class="[props.class, rightAlignClass]" :style="rightStyle">
          <slot name="lowerright" />
        </div>
      </template>
    </div>
    <div :class="[bottomAlignClass, 'align-bottom']">
      <slot name="bottom" />
    </div>
  </div>
</template>