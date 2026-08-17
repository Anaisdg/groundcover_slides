<script setup lang="ts">
defineProps<{
  items: { title: string; detail?: string }[]
}>()
</script>

<template>
  <div class="gc-steps">
    <div v-for="(s, i) in items" :key="s.title" class="gc-step">
      <div class="gc-step__n">{{ String(i + 1).padStart(2, '0') }}</div>
      <div class="gc-step__t">{{ s.title }}</div>
      <div v-if="s.detail" class="gc-step__d">{{ s.detail }}</div>
    </div>
  </div>
</template>
