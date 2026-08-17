<template>
  <div class="slidev-layout gc-frame">
    <header class="gc-header">
      <img class="gc-logo" src="/logo.png" alt="groundcover" />
      <div class="gc-meta">
        <span class="gc-num">
          {{ String($slidev.nav.currentPage).padStart(2, '0') }} / {{ String($slidev.nav.total).padStart(2, '0') }}
        </span>
      </div>
    </header>
    <div class="gc-statement">
      <h2 class="gc-statement__h" v-html="$frontmatter.headline" />
      <p v-if="$frontmatter.sub" class="gc-sub" style="margin-top:20px" v-html="$frontmatter.sub" />
      <div v-if="$frontmatter.cta" style="margin-top:30px">
        <span class="gc-pill--dark">{{ $frontmatter.cta }}</span>
      </div>
    </div>
    <slot />
  </div>
</template>
