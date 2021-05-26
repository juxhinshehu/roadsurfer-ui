<template>
  <div id="app">
    <Navigation/>
    <div class="container">
      <Availabilities/>
    </div>
  </div>
</template>

<script>
import Availabilities from './components/Availabilities.vue'
import Navigation from './components/Navigation.vue'

export default {
  name: 'App',
  components: {
    Availabilities,
    Navigation
  }
}
</script>

