<template>
  <div id="board">
    <StartScreen v-if="!players" @start="p => setPlayers(p)"/>
      <div style="height:inherit" v-else>


        <div v-for="planet in ['1', '2', '3', '4']" :key="planet">
          <img src="@/assets/planet1.png">
        </div>

        <div id="player-row" class="d-flex justify-content-around">
          <div class="d-flex justify-content-end flex-column" v-for="player in players" :key="player">
            <p class="text-center text-success" v-if="playerTurn==player.id">Your Turn</p>
            <div class="card">
              <div class="card-header d-flex justify-content-between">
                <div class="d-flex flex-column justify-content-between">
                  <p class="m-0">{{player.name}}</p>
                  <p class="m-0">HP: {{player.health}}</p>
                </div>
                
                <div class="d-flex flex-column justify-content-between">
                  <p class="m-0">Defense: {{player.mercenaries}}</p>
                  <p class="m-0">${{player.resources}}</p>
                </div>
              </div>
              <div class="card-body">
                <button class="btn btn-outline-primary my-1 w-100" >Buy Defenses ($10)</button>
                <button class="btn btn-outline-primary my-1 w-100">Next Planet ($100)</button>
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
