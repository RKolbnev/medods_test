<template>
  <div>
    <app-header></app-header>
    <app-main></app-main>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader'
import AppMain from './components/AppMain'

export default {
  data () {
    return {}
  },
  components: {
    'app-header': AppHeader,
    'app-main': AppMain
  }
}
</script>

<style></style>
