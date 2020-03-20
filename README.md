# alligator-zest --- a repo for teaching Vue Template Syntax

## Vue Template Syntax

**Vue** is best utilized when using its templating features to the full extent possible. Take its "directives", which refers to tag attributes with the `v-` prefix. You probably directives like the following pretty often: `<a v-bind:href="url"></a>`.

With this, you could have a variable `url` in your Vue instance that your anchor tag uses as its `href`.

Dynamic arguments take your directives to a new level though. Consider the following:

`a v-bind:[attributeName]="url">...</a>`

`attributeName` is itself a Javascript expression like `url`, interpreted as such because of the square brackets around it.

Let's try it with the other directive that we will find ourselves using a lot.

`<a v-on:click:"myFunction"></a>` is how we would call one of our component's functions upon clicking the link. `<a v-on:[event]="myFunction"></a>` would mean that the event variable could be `"hover"` or `"click"` or any other attribute used with `v-on`. Let's try and use this in a nifty way.

Before we get started with that let's go over one more thing. The directives `v-on` and `v-bind` are so common that we have shortcuts for writing them in Vue; `:` and `@`. So, an `img` tag with a dynamic attribute could be `<img :[classOrId]="value" @click="display">` where `display` is a function, `value` is a string variable, and `classOrId` is also a string variable.

Today we are going to create a photo gallery with this new fangled syntax. Get ready!

---

### Coding



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
