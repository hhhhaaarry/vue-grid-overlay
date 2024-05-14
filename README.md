# Grid Overlay for Vue 3 and Nuxt 3

`Grid Overlay` is a Vue component designed to facilitate the development and design of interfaces, allowing developers and designers to visualize a grid over their content, similar to the grids available in Figma.

## Features

- **Adjustable Visibility:** Activate or deactivate the display of the grid with the keyboard shortcut `Shift + G`.
- **Complete Customization:** Adjust the number of rows and columns, colors, opacity and more, with a figma-like interface.
- **Easy Integration:** Compatible with Vue 3 and Nuxt 3 projects.

## Installation

```bash
npm install vue-grid-overlay
```

## Usage in Vue 3

First, import the `Grid Overlay` component into your view/page or `App.vue` file.


```vue
<template>
<div id="app">
<GridOverlay />
<!-- The rest of your application here -->
</div>
</template>
<script>
import { GridOverlay } from "vue-grid-overlay";
export default {
name: "App",
components: {
GridOverlay,
},
};
</script>
```


## Usage in Nuxt 3


First Create a plugin to register the `GridOverlay` component globally:

Create a file `plugins/grid-overlay.js` with the following content:

```js
import { GridOverlay } from "vue-grid-overlay";

export default defineNuxtPlugin((nuxtApp) => {
nuxtApp.vueApp.component("GridOverlay", GridOverlay);
});
```

Then Register this plugin in your `nuxt.config.js`:

```js
export default {
build: {
transpile: ['vue-grid-overlay'], // Ensure the package is transpiled
},
plugins: [
'~/plugins/grid-overlay.js'
]
}
```


## Keyboard Shortcuts

Then you can press `Shift + G` to toggle the grid overlay on and off, it has 3 states: `off`, `on`, and `on with 50% opacity`.

## Contributions

This project is open source and we welcome any contributions. Whether it's a new feature, a bug fix, or improvements to the documentation, your help is welcome.

## License

Distributed under the MIT license. See `LICENSE` for more information.

## Contact

If you have any questions or need help with the component, please open an issue in the GitHub repository.
