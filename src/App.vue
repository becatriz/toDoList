<template>
  <v-app id="app">
    <nav-bar></nav-bar>
    <v-main>
      <v-container>
        <home></home>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>

import NavBar from './components/NavBar.vue'
import Home from './components/Home.vue'

export default {
  name: 'App',
  components: {
    NavBar,
    Home
  }
}
</script>

<style>

</style>
