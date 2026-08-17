<script setup lang="ts">
defineProps<{
  columns: string[]
  rows: { label: string; cells: string[] }[]
  winner?: number
}>()
</script>

<template>
  <table class="gc-table">
    <thead>
      <tr>
        <th />
        <th v-for="c in columns" :key="c">{{ c }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="r in rows" :key="r.label">
        <td style="font-weight:600">{{ r.label }}</td>
        <td
          v-for="(cell, i) in r.cells"
          :key="i"
          :class="winner !== undefined ? (i === winner ? 'gc-win' : 'gc-lose') : ''"
        >
          {{ cell }}
        </td>
      </tr>
    </tbody>
  </table>
</template>
