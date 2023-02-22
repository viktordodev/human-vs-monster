<template>
  <div>
    <header>
      <h1>Monster Game</h1>
    </header>
    <GameLayout/>
  </div>
</template>

<script>
import GameLayout from './components/Game_Layout.vue'

export default {
  name: 'App', 
  components: {
    GameLayout
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
header {
  background-color: darkred;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 10vh;
}
h1 {
  text-align: center;
  color: white;
  font-weight: bold;
  font-size: 3.5rem;
  display: inline-block;
}
</style>
