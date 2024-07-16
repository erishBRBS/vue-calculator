<template>
  <div id="app" class="app-container">
    <calculator></calculator>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>

<style scoped>
.app-container {
  background-color: #2e2e2e; /* Dark gray background color */
  min-height: 100vh; /* Ensure it covers the full viewport height */
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
