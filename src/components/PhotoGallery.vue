<template>
  <div>
    <ul class="gallery">
      <li v-for="n in 5" :key="n">
        <img
          @click="highlight"
          :src="require('@/assets/photos/beijing/' + n + '.jpeg')"
        >
      </li>
    </ul>
    <div id="frame">
      <img
        :src="this.theatrical"
      >
    </div>
  </div>
</template>

<script>
export default {
  name: 'PhotoGallery',
  data() {
    return {
      theatrical: ""
    }
  },
  methods: {
    highlight() {
      event.target.id = "theater";
      this.theatrical = event.target.src;
      let eventIterator = event.target.parentNode;
      while (eventIterator.previousElementSibling != null) {
        eventIterator.previousElementSibling.getElementsByTagName('img')[0].id = "";
        eventIterator = eventIterator.previousElementSibling;
      }
      eventIterator = event.target.parentNode;
      while (eventIterator.nextElementSibling != null) {
        eventIterator.nextElementSibling.getElementsByTagName('img')[0].id = "";
        eventIterator = eventIterator.nextElementSibling;
      }
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.gallery {
  display: flex;
  justify-content: space-around;
}
#frame img {
  width: 80%;
}
img {
  width: 20%;
}
#theater {
  width: 40%;
}
</style>
