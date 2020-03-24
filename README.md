# alligator-zest --- a repo for teaching Vue Template Syntax

![screenshot.png](screenshot.png)

## Vue Template Syntax

**Vue** is best used when using its templating features. It becomes very intuitive to build fancy user interfaces.

Take its "directives", which refers to tag attributes with the `v-` prefix.

You could have a variable `url` in your Vue instance that your anchor tag uses as its `href`. That would look like this:

`<a v-bind:href="url"></a>`

Let's try it with the other directive that we will find ourselves using a lot.

`<a v-on:click:"myFunction"></a>` is how we would call one of our component's functions upon clicking the link.

Dynamic arguments take your directives to a new level. Consider the following:

`a v-bind:[attributeName]="url">...</a>`

`attributeName` is itself a Javascript expression like `url`, interpreted as such because of the square brackets around it.

 `<a v-on:[event]="myFunction"></a>` would mean that the event variable could be `"hover"` or `"click"` or any other attribute used with `v-on`.

Let's go over one more thing. The directives `v-on` and `v-bind` are so common that we have shortcuts for writing them in Vue; `:` and `@`. So, an `img` tag with a dynamic attribute could be `<img :[classOrId]="value" @click="display">` where `display` is a function, `value` is a string variable, and `classOrId` is also a string variable.

Today we are going to create a photo gallery with some of this new fangled syntax. Get ready!

---

### Coding

## Setup

Start by running either
```bash
$ npm install -g @vue/cli
```
or
```bash
yarn global add @vue/cli
```
in your Terminal.

Now you will be able to run the `vue` command from the command line. Let us create a Vue application called alligator-test.
```bash
vue create alligator-test
```

---

We're going to change `HelloWorld.vue` to be `PhotoGallery.vue`. `App.vue` should look something like this:

```vuejs
<template>
  <div id="app">
    <PhotoGallery/>
  </div>
</template>

<script>
import PhotoGallery from './components/PhotoGallery.vue'

export default {
  name: 'App',
  components: {
    PhotoGallery
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
```

`PhotoGallery.vue` is where we're about to get fancy while keeping things simple at the same time.

## Building our Gallery

<p class="file-desc"><span>PhotoGallery.vue</span></p>

```vuejs
<template>
  <div>
    <ul class="gallery">
      <li v-for="n in 5" :key="n">
        <img
          :src="require('@/assets/photos/beijing/' + n + '.jpeg')"
        >
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'PhotoGallery'
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
.gallery {
  display: flex;
  justify-content: space-around;
}
img {
  width: 20%;
}
</style>
```

This gives us a basic photo gallery, assuming you have 5 photo files in the `assets/photos/beijing` folder named `1.jpeg` through `5.jpeg`. The `@` symbol is a Webpack alias that points to the `src` folder. Note the `display: flex` on `"gallery"` as well as the `v-for` in the `<li>` tag. Let's update this code so that when we click on a photo it is enlarged.

<p class="file-desc"><span>PhotoGallery.vue</span></p>

```vuejs
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
  </div>
</template>

<script>
export default {
  name: 'PhotoGallery'
},
methods: {
  highlight() {
    event.target.id = "theater";
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
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
.gallery {
  display: flex;
  justify-content: space-around;
}
img {
  width: 20%;
}
#theater {
  width: 40%;
}
</style>
```

We added a `v-on:click` to each image firing the `highlight()` method. This method makes the image you click on enlarge while keeping the others thumbnail sized. How does it do it? It sets the `id` of the clicked image to `"theater"` which has a larger width. Then, it gets the sibling nodes of the parent node of the image, the `li` with the `v-for` in it. It goes into all of these `li` tags and sets their respective `img` tag's `id` to a null string to make sure that only one `img` has the `"theater"` `id` at any given time.

This is cool but it is still not  what we see on the web; how can we get the enlarged image to be a big display, say under the 5 thumbnails? The end result would be a roll of thumbnails with the selected image enlarged to a real theater size right below.

We're going to add the following:

```html
<div id="frame">
  <img
    :src="this.theatrical"
  >
</div>
```
<pre><code class="javascript">
data() {
  return {
    theatrical: ""
  }
},
methods: {
	highlight() {
    	event.target.id = "theater";
    	<span class="code-annotation">this.theatrical = event.target.src;</span>
</code></pre>

And finally, add the following to your CSS.

<pre><code class="css">
#frame img {
  width: 80%;
}
</code></pre>

Now you have a nice gallery view of your photos! All with some nifty use of Vue's reactivity system, data properties, template syntax, 

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
