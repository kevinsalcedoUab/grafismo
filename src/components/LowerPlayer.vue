<template>
  <div>
      <p v-if="state">{{playerInfo}}</p>
  </div>
</template>

<script>
export default {
    name: 'LowerPlayer',
    props: ['pID'],
    data: function(){
        return {
            playerInfo: {},
            maxMetrics: {
                kda: 0,
                csMin: 0,
                goldMin: 0,
                dmgMin: 0,
            },
            time: 0,
            state: true,
            competitionID: 'a3141008-3e14-11eb-892c-065e1d3d7cd4',
            playerID: '0b7c856e-15e0-11e9-88c3-22000ac39d70',
        }
    },
    created: function(){
        
    },
    watch: {
        pID: function(){
            console.log("paso: ", this.pID);
            fetch("https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/"+ this.competitionID+"/players/"+ this.pID)
            .then(response => response.json())
            .then(data => {
                const infoPlayer = data.data;
                this.saveInfoPlayer(infoPlayer);
            });
            //setInterval(() => this.time++, 10000);
        },
        time: function(){
            if(this.state==1){
                this.state = false;
            }
        },
    },
    methods: {
        saveInfoPlayer(infoPlayer){
            let prova = {
                nickname: infoPlayer.player.nickname,
                kda: infoPlayer.metrics.kda,
                cdMin: infoPlayer.metrics.creepsPerMinute,
                goldMin: infoPlayer.metrics.goldPerMinute,
                dmgMin: infoPlayer.metrics.damagePerMinute,
                champ:  infoPlayer.metrics.champions ? infoPlayer.metrics.champions[0].champion.name : null,
                team: infoPlayer.team.name,
                photo: infoPlayer.player.photo.original,
            }
            this.playerInfo = prova;
        },
    },
}
</script>

