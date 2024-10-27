<script setup>
import { onMounted, ref } from 'vue'
import { PhotoAlbum } from 'vue-photo-album'
import usePhotoSwipe from './usePhotoSwipe.ts'
import CustomPhotoSwipeAdapter from './CustomPhotoSwipeAdapter.vue'

import { Reveal } from 'vue3-burger-menu' // import the CSS transitions you wish to use, in this case we are using `Slide`

import titleImage from './tubbycreativeblack.png'

import { SGithub } from 'vue-socials'

// const access_token = 'IGQVJVX3ZAPVEpjV0R3TmU1SzBoN0JFUWxRUHk0REJTekNYeWxkSXVDT0VlTDVGR2JldVd4VUJUb1JjV2h1MlZAad1FwWTN2VUo2TTB6eDRHOHZAZAc19VWmM4ZAjdkRHlxT3hKMkpnYmtBSktxc2ZArUVk1ZAwZDZD'
// const url = 'https://graph.instagram.com/me/media?fields=id,username,caption,media_url'
// const username = 'btubbz'
usePhotoSwipe({ gallery: '#demo', children: 'a' })

const images = ref([])
const count = ref(0)
const columns = ref(3)
const spacing = ref(5)
const titleOpacity = ref(1)

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

const listenScrollEvent = e => {
  if (window.scrollY > 100) {
    console.log('0.9')
    titleOpacity.value = 0.9
  }
  if (window.scrollY > 200) {
    console.log('0.7')
    titleOpacity.value = 0.7
  }
  if (window.scrollY > 400) {
    console.log('0.5')
    titleOpacity.value = 0.5
  }
  if (window.scrollY > 600) {
    titleOpacity.value = 0.3
  }
  // console.log(`Y: ${window.scrollY} opacity: ${opacity}`);
  // this.setState({ opacity: opacity })

  // Implement infinite scroll
  const bottom =
    Math.ceil(window.innerHeight + window.scrollY) >=
    document.documentElement.scrollHeight
  if (bottom) {
    // console.log(`at the bottom ${this.state.nextpage}`);
    this.__fetchInsta(this.state.nextpage)
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

  window.addEventListener('scroll', listenScrollEvent)
  // document
  //   .getElementsByClassName('bm-menu')[0]
  //   .addEventListener('click', function (event) {})
})
</script>

<template>
  <Reveal>
    <span>Visualiser</span>
    <br />
    <span>Bass Guitarist</span>
    <br />
    <span>Call Will on 07989742643</span>
  </Reveal>

  <main id="page-wrap">
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
  </main>
</template>

<style>
.bm-menu {
  z-index: 1100 !important;
}
.bm-burger-bars {
  background: #e94375 !important;
}

.bm-burger-button {
  z-index: 1000;
  top: 45px !important;
}
.view {
  margin: 0 auto;
  /* padding: 1rem; */
  /* max-width: 1200px; */
}
</style>

<style scoped>
#title_container {
  position: fixed;
  top: 20px;
  right: 0px;
  z-index: 100;
  height: 110px;
  text-align: center;
}
#title-image {
  width: 80%;
  opacity: v-bind('titleOpacity');
}

@media (min-width: 1024px) {
}
</style>
