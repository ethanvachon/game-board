<template>
  <div id="board">
    <StartScreen v-if="!players" @start="p => setPlayers(p)"/>
      <div style="height:inherit" v-if="players && gameWin == null">
        <div class="card p-2" style="width:fit-content">
          <h2 class="m-0">Turn {{turns}}</h2>
        </div>

        <div class="d-flex justify-content-center" v-if="viewDice">
          <div class="card" id="roll-dice">
            <div class="d-flex justify-content-around my-2" v-if="diceRolling">
              <i class="fas fa-dice-five fa-spin"></i>
              <i class="fas fa-dice-five fa-spin"></i>
            </div>
            <div class="d-flex flex-column text-center" v-else>
              <h2 class="my-2">Got ${{addResource}}</h2>
              <button class="btn btn-primary my-2" @click="closeDice()">Accept</button>
            </div>
          </div>
        </div>

        <div class="d-flex justify-content-center" v-if="showEvent">
          <div class="card" id="event">
            <div class="d-flex my-2">
              <h2 class="m-0">{{eventMessage}}</h2>
            </div>
            <div class="d-flex justify-content-center">
              <button class="btn btn-primary my-2" @click="closeEvent()">Accept</button>
            </div>
          </div>
        </div>

        <div class="d-flex justify-content-around align-items-end" id="planet-row">
          <div class="d-flex flex-column">
            <div v-for="player in planet1" :key="player">
              <p class="my-1 text-center text-white">{{players.find(p=>p.id==player).name}}</p>
            </div>
            <img src="@/assets/planet1.png">
          </div>
          
          <div class="d-flex flex-column">
            <div v-for="player in planet2" :key="player" class="">
              <p class="my-1 text-center text-white">{{players.find(p=>p.id==player).name}}</p>
            </div>
            <img src="@/assets/planet2.png">
          </div>
          
          <div class="d-flex flex-column">
            <div v-for="player in planet3" :key="player" class="">
              <p class="my-1 text-center text-white">{{players.find(p=>p.id==player).name}}</p>
            </div>
            <img src="@/assets/planet3.png">
          </div>
          
          <div class="d-flex flex-column">
            <div v-for="player in planet4" :key="player" class="">
              <p class="my-1 text-center text-white">{{players.find(p=>p.id==player).name}}</p>
            </div>
            <img src="@/assets/planet4.png">
          </div>

          <img src="@/assets/finish.png">
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
                <button class="btn btn-outline-primary my-1 w-100" :disabled="playerTurn==player.id && (viewDice==false&&showEvent==false) ? false : true" @click="buyDefenses()">Buy Defenses ($25)</button>
                <button class="btn btn-outline-primary my-1 w-100" :disabled="playerTurn==player.id && (viewDice==false&&showEvent==false) ? false : true" @click="upgradeMining()">Upgrade Mining ($100)</button>
                <button class="btn btn-outline-primary my-1 w-100" :disabled="playerTurn==player.id && (viewDice==false&&showEvent==false) ? false : true" @click="nextPlanet()">Next Planet ($500)</button>
                <button class="btn btn-outline-primary my-1 w-100" :disabled="playerTurn==player.id && (viewDice==false&&showEvent==false) ? false : true" @click="nextTurn()">End Turn</button>
              </div>
            </div>
          </div>
        </div>

      </div>
      <div id="game-win" class="text-center w-100" v-if="gameWin!=null">
        <h1>{{gameWin}}</h1>
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
      gameWin: null,
      diceRolling: false,
      viewDice: false,
      addResource: null,
      showEvent: false,
      turns: 0,
      players: null,
      playerTurn: 0,
      eventMessage: null,
      planet1: [],
      planet2: [],
      planet3: [],
      planet4: [],
    }
  },
  methods: {
    setPlayers(p) {
      this.players = p
      for(let i = 0; i < p.length; i++) {
        this.planet1.push(p[i].id)
      }
      this.nextTurn()
    },
    nextTurn() {
      if(this.playerTurn == this.players.length) {
        this.playerTurn = 1
      } else {
        this.playerTurn++
      }
      if(this.players.filter(p => p.health != 0).length == 0) {
        this.gameWin = "everyone lost!!!"
      }
      const player= this.players.find(p=>p.id==this.playerTurn);
      if(player.health==0) {
        this.nextTurn()
        return
      }
      this.turns++
      this.rollDice()
    },
    rollDice() {
      this.viewDice = true
      this.diceRolling = true
      var self = this
      const player= this.players.find(p=>p.id==this.playerTurn);
      setTimeout(function() {
        self.addResource = Math.floor((Math.random() * 200 + 51) * player.multiplier);
        self.diceRolling = false
      }, 2000);
    },
    closeDice () {
      this.players.find(p=>p.id==this.playerTurn).resources += this.addResource
      this.addResource = null
      this.viewDice = false
      this.triggerEvent()
    },
    upgradeMining () {
      const player= this.players.find(p=>p.id==this.playerTurn);
      if(player.resources >= 100) {
        player.resources -= 100
        player.multiplier += 0.1
      }
    },
    nextPlanet () {
      const player= this.players.find(p=>p.id==this.playerTurn);
      if(player.resources >= 500) {
        player.resources -= 500
        for(let i = 1; i <= 4; i++) {
          if(this[`planet${i}`].filter(p => p==player.id).length == 1) {
            if(i==4) {
              this.gameWin = `${player.name} won!`
            } else {
              this[`planet${i}`] = this[`planet${i}`].filter(p=>p!=player.id)
              this[`planet${i+1}`].push(player.id)
            }
            break;
          }
        }
      }
    },
    buyDefenses() {
      const player= this.players.find(p=>p.id==this.playerTurn);
      if(player.resources >= 25) {
        player.resources -= 25
        player.mercenaries += 1
      }
    },
    triggerEvent () {
      const player = this.players.find(p=>p.id==this.playerTurn);
      let eventId = Math.floor(Math.random() * 15)  + 1
      if(eventId <= 5) {
        return;
      } else if(eventId >= 12) {
          this.showEvent = true
          if(player.mercenaries >= 5) {
                player.mercenaries -= 5
                this.eventMessage = "you were attacked by pirates! lost defences"
              } else {
                if(player.health == 1) {
                  player.health -= 1
                  this.eventMessage = "you were attacked by pirates and don't have health! you are out of the game"
                } else {
                  player.mercenaries = 0
                  player.health -= 1
                  this.eventMessage = "you were attacked by pirates! your defences weren't sufficient and you lost health!"
                }
              }
          this.eventMessage = "pirates!!"
      } else {
        this.showEvent = true
        switch (eventId) {
          case 6: 
          for(let i = 1; i <= 4; i++) {
              if(this[`planet${i}`].filter(p => p==player.id).length == 1) {
                if(i !=1) {
                  this[`planet${i}`] = this[`planet${i}`].filter(p=>p!=player.id)
                  this[`planet${i-1}`].push(player.id)
                }
                break;
              }
            }
            this.eventMessage = "lost planet"
            break;
          case 7: 
            player.resources -= 100
            this.eventMessage = "you lost materials!"
            break;
          case 8: 
            player.multiplier += 0.1;
            this.eventMessage = "you found a way to streamline your mining process!"
            break;
          case 9: 
            for(let i = 1; i <= 4; i++) {
              if(this[`planet${i}`].filter(p => p==player.id).length == 1) {
                if(i==4) {
                  this.gameWin = `${player.name} won!`
                } else {
                  this[`planet${i}`] = this[`planet${i}`].filter(p=>p!=player.id)
                  this[`planet${i+1}`].push(player.id)
                }
                break;
              }
            }
            this.eventMessage = "found enough fuel to make it to the next planet!"
            break;
          case 10: 
            this.eventMessage = "found 100 resources!"
            player.resources += 100
            break;
          case 11: 
            this.eventMessage = "found 100 resources!"
            player.resources += 100
            break;
        }
      }
    },
    closeEvent () {
      this.showEvent = false
      this.eventMessage = null
    },
  },
}
</script>

<style>
#game-win {
  position: absolute;
  top: 40vh;
  color: white;
}

#planet-row {
  position: relative;
  top: 20vh;
}

#board {
  height: 100vh;
  width: 100vw;
  background-image: url(./assets/background.png);
}

img {
  height: 10em;
  width: auto;
}

#player-row {
  position: absolute;
  bottom: 0px;
  width: 100vw;
  padding: 3em;
}

#roll-dice {
  width: 15em;
  position: absolute;
  top: 30vh;
  z-index: 100;
}
 
#event {
  width: 15em;
  position: absolute;
  top: 30vh;
  z-index: 100;
}

.card {
  width: 17em;
  border-radius: 0px !important;
  border-width:2px !important;
}

.fa-dice-five {
  font-size: 3em;
}
</style>
