<template>
  <div class="m-auto w-1/2">
    <ImageSlider
      v-bind:slides="slides"
    />
  </div>
</template>

<script>
import ImageSlider from './components/ImageSlider.vue'

export default {
  name: 'App',
  data () {
    return {
      slides: [
        'https://source.unsplash.com/random/800x400?i=1',
        'https://source.unsplash.com/random/800x400?i=2',
        'https://source.unsplash.com/random/800x400?i=3',
        'https://source.unsplash.com/random/800x400?i=4',
        'https://source.unsplash.com/random/800x400?i=5',
        'https://source.unsplash.com/random/800x400?i=6',
      ]
    }
  },
  components: {
    ImageSlider
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
