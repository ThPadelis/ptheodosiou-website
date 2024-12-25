---
publishDate: 2022-10-19T09:30:00Z
# author: Pantelis Theodosiou
title: Vue 3 state to your CSS with v-bind()
excerpt: Learn how to dynamically apply Vue 3 state to your CSS using the v-bind() directive, making your applications more interactive and responsive.
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
# category: Tutorials
tags:
  - vuejs
  - javascript
  - fontend-development
# metadata:
#   canonical: https://astrowind.vercel.app/get-started-website-with-astro-tailwind-css
---

To make the application more dynamic, Vue allows us to connect dynamic state to markup inside the template. 

For instance, you could only want to apply a class to an element if a specific criterion is met. But did you know that component state may also be directly applied to a CSS property in the style tag?

The style tag is compatible with the CSS function known as `v-bind()` which enables dynamic CSS property values. Let's examine the topic at hand in more detail.

```vue
<template>
  <div class="custom-wrapper">
    <Box v-for="i in 35" :key="i" />
  </div>  
</template>

<script setup>
import Box from "./components/Box.vue"
</script>

<style scoped>
.custom-wrapper {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  max-width: 1260px;
  gap: 10px;
}
</style>
```

The code for the main part, named `App.vue` is seen above. You can see the code for the `Box.vue` component, which is being imported into the `App.vue` file as part of this component, below.

```vue
<template>
    <div class="custom-box"></div>
</template>

<style scoped>
.custom-box {
    display: inline-flex;
    height: 50px;
    width: 50px;
    background: gray;
}

.is-done {
    background: green;
}
</style>
```

If we look at this component, it is fairly straightforward. To make this box, we simply have a `div` with the class `custom-box` and some styles. Let's see how we can provide this component some dynamic CSS attribute values.

We must first establish a new property named `color`. This property will have the string value `green` as its default value.

```vue
<script setup>
defineProps({
    color: {
        type: String,
        default: "green"
    },
})
</script>
```

Now, we can utilize this `color` attribute within the style tag and assign it to the background color property as seen below.

```vue
<style scoped>
/* code omitted */
.is-done {
    background: v-bind(color); /* previous value: "green" */
}
</style>
```

As a result, we can now utilize the `v-bind()` method and pass in the `color` property rather than having a static value of `green`. After that, the `Box` component will dynamically receive the color we are supplying as a property. 

The CSS will remain static since the real value will be assembled into a hashed custom property. Inline styles will be used to apply the custom property to the component's root element, and it will be reactively updated if the source value changes.

![](/home/ptheodosiou/Dropbox/Articles/vue3-state-tocss/custom-color.png)

### Conclusion

The `v-bind()` CSS function is supported by single file component `<style>` tags for connecting CSS values to dynamic component state. It is a fun and easy way to keep reactivity in Vue components.
