<template>
  <div class="slidev-layout gc-frame">
    <header class="gc-header">
      <img class="gc-logo" src="/logo.png" alt="groundcover" />
      <div class="gc-meta">
        <span v-if="$frontmatter.eyebrow">{{ $frontmatter.eyebrow }}</span>
        <span class="gc-num">
          {{ String($slidev.nav.currentPage).padStart(2, '0') }} / {{ String($slidev.nav.total).padStart(2, '0') }}
        </span>
      </div>
    </header>

    <h2 v-if="$frontmatter.title" class="gc-title">{{ $frontmatter.title }}</h2>
    <p v-if="$frontmatter.sub" class="gc-sub" v-html="$frontmatter.sub" />

    <div class="gc-body" :style="{ marginTop: $frontmatter.title ? '24px' : '0' }">
      <slot />
    </div>
  </div>
</template>
