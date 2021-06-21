<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <table-players/>
    <lower-player/>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import LowerPlayer from './components/LowerPlayer.vue'
import TablePlayers from './components/TablePlayers.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    LowerPlayer,
    TablePlayers
  }
}
</script>

