<template>
  <div id="app">
    <player-init v-if="isNotDone" @getPlayers="getPlayers($event)"></player-init>
    <assaign-targets v-if="isAssigning" @doneAssigning="doneAssigning" :players="players"></assaign-targets>
    <game-board v-if="isSetupDone" :players="players"></game-board>
  </div>
</template>

<script>
import PlayerInit from "./player-init.vue"
import AssaignTargets from "./assign-targets.vue"
import gameBoard from "./game-board.vue"
export default {
    name: 'app',
    data () {
        return {
            players: [],
            isNotDone: true,
            isAssigning: false,
            isSetupDone: false
        }
    },
    methods: {
        getPlayers(playersArr) {
            // console.log('playersArr', playersArr);
            
            this.players = playersArr;
            this.isNotDone = false;
            this.isAssigning = true;
        },
        doneAssigning() {
          this.isAssigning = false;
          this.isSetupDone = true;
        }
    },
    components: {
            PlayerInit,
            AssaignTargets,
            gameBoard
    },
    // watch: {
    //     players(){
    //         console.log('players', this.players);
    //     }
    // }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
