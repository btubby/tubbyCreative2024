<script setup>
import { onMounted, ref } from 'vue'
import { PhotoAlbum } from 'vue-photo-album'
import usePhotoSwipe from './usePhotoSwipe.ts'
import CustomPhotoSwipeAdapter from './CustomPhotoSwipeAdapter.vue'

import titleImage from './tubbycreativeblack.png'

// const access_token = 'IGQVJVX3ZAPVEpjV0R3TmU1SzBoN0JFUWxRUHk0REJTekNYeWxkSXVDT0VlTDVGR2JldVd4VUJUb1JjV2h1MlZAad1FwWTN2VUo2TTB6eDRHOHZAZAc19VWmM4ZAjdkRHlxT3hKMkpnYmtBSktxc2ZArUVk1ZAwZDZD'
// const url = 'https://graph.instagram.com/me/media?fields=id,username,caption,media_url'
// const username = 'btubbz'
const images = ref([])
usePhotoSwipe({ gallery: '#demo', children: 'a' })

const count = ref(0)
const columns = ref(3)
const spacing = ref(5)
const url =
  'https://api.flickr.com/services/rest/?method=flickr.photos.search' +
  '&api_key=52f7e77ccc225a2c6cda920bb6173fb5' +
  '&user_id=tubbycreative' +
  '&sort=date-taken-desc' +
  // '&tag_mode=all' +
  '&extras=tags,date_upload,date_taken,media,url_n,url_l,url_z,url_o' +
  '&per_page=100' +
  '&page=1' +
  '&format=json' +
  '&nojsoncallback=1'

// const onClick = payload => {
//   console.log(payload.photo)
// }

const processFickrImagesOrVideo = entry => {
  return {
    media: entry.media,
    id: entry.id,
    src: entry.url_z,
    hires: entry.url_l,
    height: parseInt(entry.height_l),
    width: parseInt(entry.width_l),
    datetaken: entry.datetaken,
    date_upload: entry.date_upload,
  }
}

onMounted(() => {
  fetch(url)
    .then(res => res.json())
    .then(result => {
      console.log(result.photos.photo)
      images.value = result.photos.photo.map(processFickrImagesOrVideo)
      console.log(images.value.length)
    })
})
</script>

<template>
  <div id="title_container">
    <img id="title-image" :src="titleImage" />
  </div>
  <div class="view">
    <PhotoAlbum
      id="demo"
      layout="columns"
      :photos="images"
      :photo-renderer="CustomPhotoSwipeAdapter"
      :spacing="spacing"
      v-if="images.length"
    />
  </div>
</template>

<style scoped>
.view {
  margin: 0 auto;
  /* padding: 1rem; */
  /* max-width: 1200px; */
}
</style>

<style scoped>
@media (min-width: 1024px) {
}
</style>
