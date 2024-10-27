<script setup lang="ts">
import { computed } from 'vue'
import { type PhotoRendererMetadata } from 'vue-photo-album'

const props = defineProps<PhotoRendererMetadata>()
const largePhoto = computed(() => props.photo.srcSet?.[0] ?? props.photo)
</script>

<template>
  <a
    :href="largePhoto.src"
    :data-pswp-width="largePhoto.width"
    :data-pswp-height="largePhoto.height"
    :data-pswp-srcset="imageProps.srcset"
    target="_blank"
    rel="noopener noreferrer"
  >
    <!-- <span class="photo-meta">
      <a href="{{largePhoto.src}}">{{ largePhoto.datetaken.split(' ')[0] }}</a>
    </span> -->
    <slot />
  </a>
</template>
