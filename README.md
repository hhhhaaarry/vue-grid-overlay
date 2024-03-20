# Grid Overlay for Vue 3 and Nuxt 3

`Grid Overlay` is a Vue component designed to facilitate the development and design of interfaces, allowing developers and designers to visualize a grid over their content, similar to the grids available in Figma.

## Features

- **Adjustable Visibility:** Activate or deactivate the display of the grid with the keyboard shortcut `Shift + G`.
- **Complete Customization:** Adjust the number of rows and columns, colors, opacity and more, with a figma-like interface.
- **Easy Integration:** Compatible with Vue 3 and Nuxt 3 projects.

## Installation

bash
npm install your-package-name-grid-overlay

## Usage

First, import the `Grid Overlay` component into your view/page or app.vue file.
Add `GridOverlay` to your `App.vue` to make it available throughout your application.
<template>
<div id="app">
<GridOverlay />
<!-- The rest of your application here -->
</div>
</template>
<script>
import GridOverlay from 'your-package-name-grid-overlay';
export default {
name: 'App',
components: {
GridOverlay
}
}
</script>
