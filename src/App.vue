<template>
  <div id="board">
    <StartScreen />
  </div>
</template>

<script>
import StartScreen from '@/components/StartScreen.vue'
export default {
  name: 'App',
  components: {
    StartScreen,
  }
}
</script>

<style>
#board {
  height: 100vh;
  width: 100vw;
  background-image: url(./assets/background.png);
}


</style>
