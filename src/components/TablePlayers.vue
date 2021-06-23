<template>
    <div>
        <table>
            <thead>
                <tr>
                    <th  v-for="(team, index) in listTeams" :key="index">{{team}}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(column, indexColumn) in listTeams" :key="indexColumn">
                    <td v-for="(team, indexTeams) in teams" :key="indexTeams" 
                    v-on:click="showInfoPlayer(team.players[indexColumn])">
                    {{team.players[indexColumn]}}
                    </td>
                </tr>
            </tbody>
            
        </table>
    </div>

</template>

<script>
//SANDER 42
//KDA: kda 4  --- 10.1
//CS/MIN: creepsPerMinute 6.9 --- 9.3
//GOLD/MIN goldPerMinute 363.8  --- 465.4
//DMG/MIN damagePerminute 371.1  --- 699.2
export default {
    name: 'TablePlayers',
    data: function(){
        return {
            maxMetrics: {
                kda: 0,
                csMin: 0,
                goldMin: 0,
                dmgMin: 0,
            },
            players: {},
            teams: [], //name, id, players[nickname]
            listTeams: [],
            competitionID: 'a3141008-3e14-11eb-892c-065e1d3d7cd4',
        }
    },
    created: function(){
        fetch("https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/"+this.competitionID+"/players")
        .then(response => response.json())
        .then(data => {
                const players = data.data;
                this.getMaxMetrics();
                this.savePlayersOnTeam(players);
            }
        );
    },
    methods: {
        showInfoPlayer: function(nickname){
            this.$emit('newPlayer', this.getIDfromNickname(nickname));
        },
        getIDfromNickname: function(nickname){
            return this.players[nickname]
        },
        getMaxMetrics: function(){
            fetch("https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/"+this.competitionID+"/player-metrics-ladder")
            .then(response => response.json())
            .then(data => {
                    this.maxMetrics.kda = data.data.kda;
                    this.maxMetrics.csMin = data.data.creepsPerMinute;
                    this.maxMetrics.goldMin = data.data.goldPerMinute;
                    this.maxMetrics.dmgMin = data.data.damagePerMinute;
            })
        },
        addPlayerToTeam: function(nameTeam, player){
            var i;
            for (i = 0; i < this.teams.length; i++) {
                if (this.teams[i].name === nameTeam) {
                    this.teams[i].players.push(player);
                }
            }
        },
        savePlayersOnTeam(players){
            players.forEach((player) => {
                let playerID = player.player.id;
                let playerNickname = player.player.nickname;
                let currentTeam = player.team.name;

                this.players[playerNickname] = playerID;
                
                if(this.listTeams.includes(currentTeam)){
                    this.addPlayerToTeam(currentTeam, playerNickname);
                }else{
                    let newTeam = {};
                    newTeam.name = player.team.name;
                    newTeam.id = player.team.id;
                    newTeam.players = [];
                    newTeam.players.push(playerNickname);
                    this.teams.push(newTeam);
                    this.listTeams.push(currentTeam);
                }
            });
        },
  },
}
</script>

