<template>
    <div>
        <div id="players-hover" @click="state = !state">PLAYERS</div>
        <transition name="table-animation">
            <table id = "tPlayers" v-if="state">
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
        </transition>
    </div>

</template>

<script>
export default {
    name: 'TablePlayers',
    data: function(){
        return {
            players: {},
            teams: [], //name, id, players[nickname]
            listTeams: [],
            competitionID: 'a3141008-3e14-11eb-892c-065e1d3d7cd4',
            state: false,
        }
    },
    created: function(){
        fetch("https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/"+this.competitionID+"/players")
        .then(response => response.json())
        .then(data => {
                const players = data.data;
                this.savePlayersOnTeam(players);
            }
        );
    },
    methods: {
        /**
        * Send ID's player selected to Lower Component 
        * @param {String} nickname
        */
        showInfoPlayer: function(nickname){
            this.$emit('newPlayer', this.getIDfromNickname(nickname));
        },
        /**
        * get ID's player of List of players
        * @param {String} nickname
        */
        getIDfromNickname: function(nickname){
            return this.players[nickname]
        },
        /**
        * Add a new player in a existing team
        * @param {String} nameTeam
        * @param {object} player
        */
        addPlayerToTeam: function(nameTeam, player){
            var i;
            for (i = 0; i < this.teams.length; i++) {
                if (this.teams[i].name === nameTeam) {
                    this.teams[i].players.push(player);
                }
            }
        },
        /**
        * Save a new player of API inside Component
        * @param {object} players
        */
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

