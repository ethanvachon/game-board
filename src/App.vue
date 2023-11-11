<template>
  <div id="board">
    <StartScreen v-if="!players" @start="p => setPlayers(p)"/>
      <div style="height:inherit" v-else>




        <div id="player-row" class="d-flex justify-content-around">
          <div v-for="player in players" :key="player">
            <p class="text-success" v-if="playerTurn==player.id">Your Turn</p>
            <div class="card">
              <div class="card-header d-flex justify-content-between align-items-end">
                <p>{{player.name}}</p>
                <p class="m-0">HP: {{player.health}}</p>
              </div>
              <div class="card-body">
                <p class="m-0">${{player.resources}}</p>
                <button class="btn btn-outline-primary" >Hire Mercenaries ($10)</button>
                <button class="btn btn-outline-primary">Next Planet ($100)</button>
              </div>
            </div>
          </div>
        </div>

      </div>
  </div>
</template>

<script>
import StartScreen from '@/components/StartScreen.vue'
export default {
  name: 'App',
  components: {
    StartScreen,
  },
  data () {
    return {
      players: null,
      playerTurn: 1,
      planet1: [],
      planet2: [],
      planet3: [],
      planet4: [],
    }
  },
  methods: {
    setPlayers(p) {
      this.players = p
    },
  },
}
</script>

<style>
#board {
  height: 100vh;
  width: 100vw;
  background-image: url(./assets/background.png);
}

#player-row {
  position: absolute;
  bottom: 0px;
  width: 100vw;
  padding: 3em;
}

.card {
  width: 17em;
  border-radius: 0px !important;
  border-width:2px !important;
}
</style>
