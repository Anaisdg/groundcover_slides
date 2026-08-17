<template>
  <div class="slidev-layout gc-frame">
    <img class="gc-logo" src="/logo.png" alt="groundcover" />
    <div class="gc-hero">
      <div>
        <h1 class="gc-hero__h1" v-html="$frontmatter.headline" />
        <p class="gc-hero__sub" v-html="$frontmatter.sub" />
        <div v-if="$frontmatter.logos" class="gc-logos">
          <span v-for="l in $frontmatter.logos" :key="l">{{ l }}</span>
        </div>
      </div>
      <div class="gc-hero__art">
        <img v-if="$frontmatter.art" :src="$frontmatter.art" alt="" />
        <div v-else class="gc-hero__art-ph">
          drop your artwork at <code>public/hero.png</code><br />then set
          <code>art: /hero.png</code>
        </div>
      </div>
    </div>
  </div>
</template>
