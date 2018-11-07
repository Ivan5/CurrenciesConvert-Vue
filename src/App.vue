<template>
  <div id="app">
    <UiConvertor />
  </div>
</template>

<script>
import UiConvertor from './components/UiConvertor.vue';

export default {
  name: 'app',
  components: {
    UiConvertor
  }
}
</script>

<style>
*{
  border-radius: 0 !important;
}

body{
  background-color: #E7F7F9;
  font-family: 'Poppins', sans-serif;
}
</style>
