<template>
    <div>
        <transition name="lower-animation">
            <div id="lowerCapsule" v-if="state">
                    <table id="lower">
                        <thead>
                        <tr>
                            <th class="col1">{{playerInfo.team}}</th><th class="col2" colspan="3">{{playerInfo.nickname}}</th><th>CAMPEÓN MÁS USADO</th>
                        </tr>                  
                        </thead> 
                    <tbody>
                        <tr>
                            <td class="col1">KDA</td> 
                            <td ><progress id="kda" :max="maxMetrics.kda" :value="playerInfo.kda"/></td> 
                            <td class="col3">{{playerInfo.kda}}</td> 
                            <td id = "photoCap" rowspan="4"><img id="playerPhoto" :src="playerInfo.photo" alt="photo"></td> 
                            <td rowspan="4"><img id="playerChamp" :src="playerInfo.champ" alt="champion"></td>
                        </tr>
                        <tr>
                            <td class="col1">CS/MIN</td> 
                            <td ><progress id="csMin" :max="maxMetrics.csMin" :value="playerInfo.csMin"/></td>
                            <td class="col3">{{playerInfo.csMin}}</td>
                        </tr>
                        <tr>
                            <td class="col1">GOLD/MIN</td> 
                            <td ><progress id="goldMin" :max="maxMetrics.goldMin" :value="playerInfo.goldMin"/></td>
                            <td class="col3">{{playerInfo.goldMin}}</td>
                        </tr>
                        <tr>
                            <td class="col1">DMG/MIN</td>
                            <td ><progress id="dmgMin" :max="maxMetrics.dmgMin" :value="playerInfo.dmgMin"/></td>
                            <td class="col3">{{playerInfo.dmgMin}}</td>
                        </tr>
                    </tbody>
                    </table>
            
            </div>
        </transition>
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
            state: false,
            competitionID: 'a3141008-3e14-11eb-892c-065e1d3d7cd4',
        }
    },
    created: function(){
        this.getMaxMetrics();
    },
    watch: {
        /**
        * Update component information everytime pID is changed
        */
        pID: function(){
            this.state = true;
            fetch("https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/"+ this.competitionID+"/players/"+ this.pID)
            .then(response => response.json())
            .then(data => {
                const pInfo = data.data;
                this.saveInfoPlayer(pInfo);
            });
            setTimeout(() => {
                this.state = false;
                this.playerInfo = {};
            }, 10000);
        },
    },
    methods: {
        /**
        * Save API info Player in Lower Component
        * @param {object} infoPlayer
        */
        saveInfoPlayer(infoPlayer){
            let newPlayer = {
                nickname: infoPlayer.player.nickname.toUpperCase(),
                kda: infoPlayer.metrics.kda,
                csMin: infoPlayer.metrics.creepsPerMinute,
                goldMin: infoPlayer.metrics.goldPerMinute,
                dmgMin: infoPlayer.metrics.damagePerMinute,
                champ:  infoPlayer.metrics.champions ? infoPlayer.metrics.champions[0].champion.images.square : 'img/noChamp.png',
                team: infoPlayer.team.name,
                photo: infoPlayer.player.photo.original,
            }
            this.playerInfo = newPlayer;
        },

        /**
        * Save API Metrics info in Lower Component
        */
        getMaxMetrics: function(){
            fetch("https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/"+this.competitionID+"/player-metrics-ladder")
            .then(response => response.json())
            .then(data => {
                    this.maxMetrics.kda = data.data.kda;
                    this.maxMetrics.csMin = data.data.creepsPerMinute;
                    this.maxMetrics.goldMin = data.data.goldPerMinute;
                    this.maxMetrics.dmgMin = data.data.damagePerMinute;
            });
        },
    },
}
</script>

