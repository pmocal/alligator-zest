<template>
  <div id="app">
    <PhotoGallery v-on:theater-mode="setBackground($event)"/>
  </div>
</template>

<script>
import PhotoGallery from './components/PhotoGallery.vue'

export default {
  name: 'App',
  components: {
    PhotoGallery
  },
  methods: {
    setBackground(color) {
      console.log(color.rgba);
      document.querySelector('body').style.backgroundColor = color.rgba;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
