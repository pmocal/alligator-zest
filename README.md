# alligator-zest

This app showcases 2 features of Vue that I wrote tutorials on: Vue template syntax and Vue custom events. Read on for the tutorials! 

If you want to see the fully styled version, visit https://alligator.io/vuejs/vue-template-syntax/.

## Screenshot

![screenshot1.png](screenshot1.png)

# Vue Template Syntax Tutorial

**Vue** is best used when using its templating features. It becomes very intuitive to build fancy user interfaces.

Take its "directives", which refers to tag attributes with the `v-` prefix.

You could have a variable `url` in your Vue instance that your anchor tag uses as its `href`. That would look like this:

`<a v-bind:href="url"></a>`

Let's try it with the other directive that we will find ourselves using a lot:

`<a v-on:click:"myFunction"></a>`

That is how we would call one of our component's functions upon clicking the link.

---

Dynamic arguments take your directives to a new level. Consider the following:

`a v-bind:[attributeName]="url">...</a>`

`attributeName` is itself a Javascript expression like `url`, interpreted as such because of the square brackets around it.

 `<a v-on:[event]="myFunction"></a>` would mean that the event variable could be `"hover"` or `"click"` or any other attribute used with `v-on`.

---

Let's go over one more thing.

The directives `v-on` and `v-bind` are so common that we have shortcuts for writing them in Vue; `:` and `@`.

So, an `img` tag with a dynamic attribute could be `<img :[classOrId]="value" @click="display">` where `display` is a function, `value` is a string variable, and `classOrId` is also a string variable.

---

Today we are going to create a photo gallery with some of this new fangled syntax. Get ready!

## Coding

### Setup

Start by running either
```bash
$ npm install -g @vue/cli
```
or
```bash
$ yarn global add @vue/cli
```
in your Terminal.

Now you will be able to run the `vue` command from the command line. Let us create a Vue application called alligator-zest.
```bash
$ vue create alligator-zest
$ cd alligator-zest
$ npm run build
$ npm run serve
```

---

We're going to change `HelloWorld.vue` to be `PhotoGallery.vue`. `App.vue` should look something like this:

<p class="file-desc"><span>App.vue</span></p>

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

### Building our gallery

Let's assume we have 5 photo files in the `assets/photos` folder named `1.jpeg` through `5.jpeg`. Use any images you want.

<p class="file-desc"><span>PhotoGallery.vue</span></p>

```vuejs
<template>
  <div>
    <ul class="gallery">
      <li v-for="n in 5" :key="n">
        <img
          :src="require('@/assets/photos/' + n + '.jpeg')"
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

<p class="info-box">The `@` symbol is a Webpack alias that points to the `src` folder.</p>

Note the `display: flex` on `"gallery"` as well as the `v-for` in the `<li>` tag. You should be able to see the app in your browser at `localhost:8080`.

Let's update this code so that when we click on a photo it is enlarged.

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

We added a `v-on:click` to each image that sets off the `highlight()` method. This method makes the image that is clicked on become larger while ensuring that the others are thumbnail sized.


<gator-collapse title="How does it do it?!">
  <p>It sets the id of the clicked image to "theater" which has a larger width. Then, it gets the sibling nodes of the parent node of the image, the li with the v-for in it. It goes into all of these li tags and sets their respective img tag's id to a null string to make sure that only one img has the "theater" id at any given time.</p>
</gator-collapse>

This is cool but it is still not  what we see on the web; how can we get the enlarged image to be a big display, say, under the 5 thumbnails? The end result would be a roll of thumbnails with the selected image enlarged to a real theater size right below. Sounds good, right?

---

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

<p class="info-box success">
	Check it out in your browser!
</p>

The big frame is filled up by the photo that you clicked on since its `src` gets set when you click. Now you have a nice gallery view of your photos!

All with some nifty use of Vue's reactivity system, data properties, and template syntax 🧪

---

# Custom Events

`v-on:click`. `v-on:hover`. We know those from [Vue Template Syntax](https://alligator.io/vuejs/vue-template-syntax/). `v-on` allows you to listen for `click` or `hover` events that occur on a particular tag and run some JS code. If `someFunction` was defined in the `methods` section of your Vue component, you could use `v-on:click=someFunction` in your template. Vue.js also allows you to listen for **custom events**, an ability which has its most important usecase in allowing child components to fire off events that parent components can listen for.

We created a photo gallery component in [Vue Template Syntax](https://alligator.io/vuejs/vue-template-syntax/). You could click any of the photos in a row of thumbnails and the photo you clicked on would be displayed in a large size below. Now, what if we wanted the background of the entire page to be set to the average color of the photo being displayed? We could call this "theater mode" for fun.

The power of **custom events** is that we can fairly easily do that. The parent component of the photo gallery, `App.vue`, simply needs to receive the average RGB value of the photo from its child component, `PhotoGallery.vue`, when the photo is clicked.

Let's get started. This tutorial picks up where the Template Syntax tutorial left off so skip the **Setup** section of this tutorial if you already did that one.

## Setup

Start by running either
```bash
$ npm install -g @vue/cli
```
or
```bash
$ yarn global add @vue/cli
```
in your Terminal.

Now you will be able to run the `vue` command from the command line. Let us create a Vue application called alligator-zest.

```bash
$ vue create alligator-zest
$ cd alligator-zest
$ npm install fast-average-color
$ npm run build
$ npm run serve
```

You should be able to see a barebones Vue app in your browser.
---
We're going to change `HelloWorld.vue` to be `PhotoGallery.vue`.

<p class="file-desc"><span>PhotoGallery.vue</span></p>

```vuejs
<template>
  <div>
    <ul class="gallery">
      <li v-for="n in 5" :key="n">
        <img
          @click="highlight"
          :src="require('@/assets/photos/' + n + '.jpeg')"
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
      var that = this;
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
```

 `App.vue` should look like this:

<p class="file-desc"><span>App.vue</span></p>

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
---

## Let's write some code

We're going to use an `npm` library called `fast-average-color` to get the average color value for a particular photo. Import it at the top of the `<script>` section of `PhotoGallery.vue`.

<pre><code class="javascript">
import from 'fast-average-color';
</code></pre>

`PhotoGallery.vue` has a method `highlight()` which is triggered when you click on one of the photos.

<pre><code class="javascript">
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
</code></pre>

This method displays the clicked photo in a larger size below the thumbnails. Notice that we used `event.target` all over the method. `event.target` is the element that was clicked on. This is the image that we want to get the average color of.

By perusing the `fast-average-color` docs, we can find the `getColorAsync` function, which returns a color object where `color.rgba` is the average RGB value. Let's go ahead and make that function call.

<pre><code class="javascript">
const fac = new FastAverageColor();
fac.getColorAsync(event.target)
    .then(function(color) {
    })
    .catch(function(e) {
        console.log(e);
    });
</code></pre>

We're not yet doing anything with `color`. Ultimately, we need to set the background color to `color.rgba` in `App.vue`. `App.vue` is going to have a method that takes `color.rgba` as a parameter. Why don't we just go ahead and write that method?

<pre><code class="javascript">
methods: {
  setBackground(rgba) {
    document.querySelector('body').style.backgroundColor = rgba;
  }
}
</code></pre>

That should look good to you!

Take a look at the template section of `App.vue`. Here is how it looks right now:

```html
<template>
  <div id="app">
    <PhotoGallery />
  </div>
</template>
```

The `App` component is going to have to pick up an event from the `PhotoGallery` component--a custom event. Say the event was called `theater-mode`; when we want to listen for such an event in our component, the syntax is just like for regular events. That is, it would be: `v-on:theater-mode`. When `theater-mode` occurs we'll call our `setBackground` method.

Now we need to send the value `color.rgba` to `App.vue` somehow. Go back to `PhotoGallery.vue`.

---

Every Vue component has a method `$emit`. This method allows you to trigger an event, in our case one called `theater-mode`. We're going to call `this.$emit` inside the `then` function from the async call we made to the color library. Let's jog your memory.

<pre><code class="javascript">
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
  const fac = new FastAverageColor();
  fac.getColorAsync(event.target)
      .then(function(color) {
      })
      .catch(function(e) {
          console.log(e);
      });
}
</code></pre>

`this.$emit` takes the event name as its first argument and has optional further arguments in which you can pass data. We will pass `color.rgba`. So our function call is going to look like `this.$emit('theater-mode', color.rgba)`.  Here's our new function:

<pre><code class="javascript">
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
  const fac = new FastAverageColor();
  fac.getColorAsync(event.target)
      .then((color) => this.$emit('theater-mode', color.rgba))
      .catch(function(e) {
          console.log(e);
      });
}
</code></pre>

That should look good to you! Let's look back at `App.vue`.

<p class="file-desc"><span>App.vue</span></p>

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
  },
  methods: {
    setBackground(rgba) {
      document.querySelector('body').style.backgroundColor = rgba;
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
```

We already discussed that listening to the `theater-mode` event looks like `v-on:theater-mode`. What should `v-on:theater-mode` `=`? Definitely `setBackground`. But how about accessing the color value that we passed in `PhotoGallery.vue`? That brings us to our final piece of syntax. When we listen for a custom event, we can access any data that is passed with it via `$event`.

So, we write the following:

```html
<template>
  <div id="app">
    <PhotoGallery v-on:theater-mode="setBackground($event)"/>
  </div>
</template>
```

## Congratulations!

Check your browser. Your app should be working just as we intended it to. Good job and bon voyage! 🚢

# Project setup
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
