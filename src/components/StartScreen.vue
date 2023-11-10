<template>
  <div class="w-100 h-100 d-flex justify-content-center align-items-center">
    <div class="card">
      <div class="card-header">
        <h5 class="m-0">Start Game</h5>
      </div>
      <div class="card-body">
        <div v-for="player in playerCount" :key="player">
          <label>Player {{player}}</label>
          <input type="text" v-model="players[player - 1].name" class="form-control-sm w-100" maxlength="20" placeholder="Name">
        </div>
        <div class="d-flex align-items-center py-2 justify-content-between">
            <button class="btn btn-outline-dark" v-on:click="addPlayer()">Add Player +</button>
            <button class="btn btn-outline-primary" v-on:click="startGame()">Start</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "StartScreen",
  data () {
    return {
      playerCount: 2,
      players: [
        {
          id: 1,
          name: null,
        },
        {
          id: 2,
          name: null,
        }
      ]
    }
  },
  methods: {
    addPlayer () {
      if(this.playerCount != 4) {
        this.players.push({
          id: this.playerCount + 1,
          name: null,
        })
        this.playerCount++
      }
    },
    startGame () {
      let valid = true
      this.players.forEach(p => {
        if(!p.name) {
          valid = false
        }
      });
      if(valid==false) {
        return;
      }
      this.$emit('start', this.players)
    },
  }
}
</script>

<style>
.card {
  width: 17em;
}
</style>
