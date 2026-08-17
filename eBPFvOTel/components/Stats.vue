<script setup lang="ts">
defineProps<{
  items: { value: string; label: string }[]
  pill?: string
}>()
</script>

<template>
  <div>
    <div class="gc-stats">
      <div v-for="s in items" :key="s.label" class="gc-stat">
        <div class="gc-stat__n">{{ s.value }}</div>
        <div class="gc-stat__l" v-html="s.label" />
      </div>
    </div>
    <div v-if="pill" class="gc-pill" style="margin-top:14px">{{ pill }}</div>
  </div>
</template>
