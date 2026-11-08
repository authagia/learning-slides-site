<script setup lang="ts">
import { computed } from 'vue'

type Stone = 'B' | 'W'

interface Props {
  size?: number
  stones?: Record<string, Stone>
  highlight?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  size: 4,
  stones: () => ({}),
  highlight: () => [],
})

const rows = computed(() =>
  Array.from({ length: props.size }, (_, row) =>
    Array.from({ length: props.size }, (_, col) => {
      const key = `${row},${col}`
      return {
        key,
        stone: props.stones[key],
        highlight: props.highlight.includes(key),
      }
    }),
  ),
)

const boardStyle = computed(() => ({
  '--board-size': props.size,
  '--cell-size': props.size >= 8 ? '1.75rem' : '2.45rem',
}))
</script>

<template>
  <div class="board-grid" :style="boardStyle" role="img" :aria-label="`${size} x ${size} board`">
    <div v-for="(row, rowIndex) in rows" :key="rowIndex" class="board-row">
      <div
        v-for="cell in row"
        :key="cell.key"
        class="board-cell"
        :class="{ 'is-highlight': cell.highlight }"
      >
        <span v-if="cell.stone" class="stone" :class="cell.stone === 'B' ? 'stone-b' : 'stone-w'" />
      </div>
    </div>
  </div>
</template>
