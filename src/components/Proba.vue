<template>
    <div> 
       
    </div>

</template>

<script>
/*
 <table>
            <thead>
                <tr>
                    <th  v-for="(team, index) in listTeams" :key="index">{{team}}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(column, indexColumn) in listTeams" :key="indexColumn">
                    <td v-for="(team, indexTeams) in teams" :key="indexTeams" v-on:click="printP(team.players[indexColumn])">{{team.players[indexColumn]}}</td>
                </tr>
            </tbody>
            
        </table>
        <table>
            <tr>
                <td v-for="(player, index) in listProba" :key="index" v-on:click="listFunction(player)">{{player}}</td>
            </tr>
        </table>


*/


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
            teams: [{
                name: 'epsilon', 
                id: 1,
                players: [{id: 'p1', nickname: 'adapting'}, {id: 'p2', nickname: 'emilito'}, {id: 'p3', nickname: 'yammin'}, {id: 'p4', nickname: 'julio'}]
                },
                {
                name: 'papis', 
                id: 2,
                players: [{id: 't1', nickname: 'warchi'}, {id: 't2', nickname: 'dheylo'}, {id: 't3', nickname: 'draco'}, {id: 't4', nickname: 'nazer'}]
                }
            ], //name, id, players[id,nickname]
            listTeams: ['epsilon', 'papis'],
            listProba: ['warchi', 'dheylo', 'julio', 'pingu'],
            competitionID: 'a3141008-3e14-11eb-892c-065e1d3d7cd4',
        }
    },
    created: function(){
        const listNewTeams = ['canival', 'pollo', 'skree', 'papis'];
        listNewTeams.forEach((team) => {
            if(this.listTeams.includes(team)){
                // console.log("El team ", team, " esta en la lista")
                this.addPlayerToTeam(team, {id: 'p5', nickname: 'knekro'})

            }else{
                // console.log("El team ", team, " NO esta en la lista");
                let newTeam = {
                name: team, 
                id: 3,
                players: [{id: 'a1', nickname: 'auron'}, {id: 'a2', nickname: 'perxita'}, {id: 'a3', nickname: 'violeta'}, {id: 'a4', nickname: 'byin'}]
                }
                this.addNewTeam(newTeam);
                this.listTeams.push(newTeam.name)
            }
        });
        // console.log("la nueva lista es esta: ", this.listTeams);
        // console.log("new mario ", this.teams);
    },
    methods: {
        printP: function(p){
            console.log(p.nickname);
        },
        listFunction: function(name){
            // console.log("la prova" + name);
            this.$emit('newPlayer', name);
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
        getTeams: function(player){
            let newPlayer = {id: player.player.id, nickname: player.player.nickname};
            let indexCurrentTeam = this.containsTeam(player.team.name)
            if(!indexCurrentTeam){
                let newTeam = {};
                newTeam.name = player.team.name;
                newTeam.id = player.team.id;
                newTeam.players = [];
                newTeam.players.push(newPlayer);
                this.teams.push(newTeam);
            }else{
                this.teams[indexCurrentTeam].players.push(newPlayer);
            }
        },
        containsTeam: function(team){
            var i;
            for (i = 0; i < this.teams.length; i++) {
                if (this.teams[i].name === team) {
                    return i
                }
            }
            return false;
        },
        addPlayerToTeam: function(nameTeam, player){
            var i;
            for (i = 0; i < this.teams.length; i++) {
                if (this.teams[i].name === nameTeam) {
                    this.teams[i].players.push(player);
                }
            }
        },
        addNewTeam: function(newTeam){
            this.teams.push(newTeam);
        }
  },
}
</script>

