<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <img src="./assets/ingame-lower-stats-player.png" alt="lower"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <TablePlayers v-on:newPlayer="playerID=$event"/>
    
    <Proba/>
    <LowerPlayer :pID = "playerID"/>

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import LowerPlayer from './components/LowerPlayer.vue'
import Proba from './components/Proba.vue'
import TablePlayers from './components/TablePlayers.vue'

export default {
  name: 'App',
  data: function(){
    return{
      playerID: '',
    }
  },
  watch: {
    playerID: function(){
      console.log("paso la sucedura: " + this.playerID);
    }
  },
  components: {
    // HelloWorld,
    LowerPlayer,
    TablePlayers,
    Proba,
  }
}
</script>

