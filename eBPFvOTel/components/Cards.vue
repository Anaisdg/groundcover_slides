<script setup lang="ts">
defineProps<{
  items: { icon?: string; title: string; points: string[] }[]
}>()
const tone = ['green', 'yellow', 'blue']
</script>

<template>
  <div class="gc-cards">
    <div
      v-for="(c, i) in items"
      :key="c.title"
      class="gc-card"
      :class="`gc-card--${tone[i % 3]}`"
    >
      <div v-if="c.icon" class="gc-card__icon">{{ c.icon }}</div>
      <h3 class="gc-card__title">{{ c.title }}</h3>
      <ul class="gc-card__list">
        <li v-for="p in c.points" :key="p" v-html="p" />
      </ul>
    </div>
  </div>
</template>
