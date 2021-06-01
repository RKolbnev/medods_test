<template>
  <div class="main">
    <div class="main-text">
      <h1>Медицинская информационная система</h1>
      <p>для медицинских центров и стоматологий</p>
      <div>МЕDODS</div>
      <button class="main-add-customer" @click="isOpen=!isOpen">Записаться на прием</button>
      <app-form
        v-if="isOpen"
        @close="isOpen=!isOpen"
      ></app-form>
    </div>
    <div class="main-bg">
      <img src="../assets/bg.png" alt="">
    </div>
  </div>
</template>

<script>
import AppForm from './AppForm'

export default {
  components: {
    'app-form': AppForm
  },
  data () {
    return {
      isOpen: false
    }
  }
}
</script>

<style></style>
