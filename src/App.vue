<template>
  <div id="app">
    <TablePlayers v-on:newPlayer="playerID=$event"/>
    <LowerPlayer :pID = "playerID"/>

  </div>
</template>

<script>
import LowerPlayer from './components/LowerPlayer.vue'
import TablePlayers from './components/TablePlayers.vue'

export default {
  name: 'App',
  data: function(){
    return{
      playerID: '',
    }
  },
  components: {
    LowerPlayer,
    TablePlayers,
  }
}
</script>

