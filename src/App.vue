<script setup>

  import {ref} from "vue";

  // Creation of all the variables needed. Unfortunately, i don´t know yet how to optimize this :(
  const score1 = ref(0)
  const score2 = ref(0)
  const game1 = ref(0)
  const game2 = ref(0)
  const set1 = ref(0)
  const set2 = ref(0)
  const maxSets = ref(0)
  const maxGames = ref(0)
  const maxTieBreak = ref(0)
  let tieBreak = false
  let softTie = false
  let isReady = false
  const names = ref({
    d1j1: '',
    d1j2: '',
    d2j1: '',
    d2j2: '',
  })

// Declarations of the methods used for most of the program functionalities.

  // Function responsible for resetting the settings on the game when a new game is started.
  function startGame() {
    console.clear()
    console.log("Game started")
    isReady = true
    softTie = false
    tieBreak = false
    score1.value = 0
    score2.value = 0
    game1.value = 0
    game2.value = 0
    set1.value = 0
    set2.value = 0
    alert(`Número de sets: ${maxSets.value}, número de games: ${maxGames.value}, máximo do tie-break: ${maxTieBreak.value}`)
  }

  // Function responsible for increasing the game (found the optimization lol)
  function increaseGame1() {
    switch (score1.value) {
      case 40:
        score1.value = 0
        score2.value = 0
        game1.value++
        console.log("Game increased")
        break;
      case maxTieBreak.value:
        score1.value = 0
        score2.value = 0
        game1.value++
        console.log("Game increased")
        break;
    }
    
  }

  // Copypaste of the function above
  function increaseGame2() {
    switch (score2.value) {
      case 40:
        score1.value = 0
        score2.value = 0
        game2.value++
        console.log("Game increased")
        break;
      case maxTieBreak.value:
        score1.value = 0
        score2.value = 0
        game2.value++
        console.log("Game increased")
        break;
    }
  }

  // Function responsible for increasing the set number
  function increaseSet1() {
    switch (game1.value) {
      case maxGames.value:
        set1.value++
        game1.value = 0
        game2.value = 0
        score1.value = 0
        score2.value = 0
        console.log("Set increased")
        tieBreak = false
        break;
    }
  }

  // Copypaste of above lol
  function increaseSet2() {
    switch (game2.value) {
      case maxGames.value:
        set2.value++
        game1.value = 0
        game2.value = 0
        score1.value = 0
        score2.value = 0
        console.log("Set increased")
        tieBreak = false
        break;
    }
  }

  // Function responsible for checking if a game is tied at maxScore. If it is, automatically adjusts for the game to end on 7x5
  function testForTie() {
    if (softTie == false && game1.value == game2.value && game1.value == maxGames.value - 1) {
      softTie = true
      console.log("The game hit a soft tie and must end by a 2 game advantage or tie-break on 6-6")
      maxGames.value++
    }

    if (softTie) {
      if (game1.value == game2.value && game1.value == maxGames.value - 1){
        tieBreak = true
        softTie = false
      }
    }

    if (score1.value == maxTieBreak.value){
      increaseGame1()
    }
    if (score2.value == maxTieBreak.value){
      increaseGame2()
    }
  }

  // Function responsible for increasing each player's points.
  function increaseScore1() {
    switch (tieBreak) {
      case true: 
      switch (score1.value) {
        case maxTieBreak.value:
          increaseSet1();
          maxGames.value--
          break;

        default:
          score1.value++
          console.log("Score increased (tie-break)")
        }
        break;

      case false:
        switch (score1.value) {
          case 0:
            score1.value = 15
            console.log("Score increased")
            break;
          
          case 15:
            score1.value = 30
            console.log("Score increased")
            break;

          case 30:
            score1.value = 40
            console.log("Score increased")
            break;

          case 40:
            increaseGame1();
            testForTie()
            break;
          
        }
    }

    increaseSet1()

    if (set1.value == maxSets.value) {
      alert("Game finished. Player 1 won.")
      startGame();
    }
  }
  
  // Pretty much a copypaste from the above function. Anyway, if I figure out how to optimize it, I'm doin it.
  function increaseScore2() {
    switch (tieBreak) {
      case true: 
        switch (score2.value) {
          case maxTieBreak.value:
            increaseSet2();
            maxGames.value--
            break;
          default: 
            score2.value++
            console.log("Score increased (tie-break)")
        }
        break;
    }
    switch (score2.value) {
      case 0:
        score2.value = 15
        console.log("Score increased")
        break;
      
      case 15:
        score2.value = 30
        console.log("Score increased")
        break;

      case 30:
        score2.value = 40
        console.log("Score increased")
        break;

      case 40:
        increaseGame2()
        testForTie()
        break;
    }

    increaseSet2()
    
    if (set2.value == maxSets.value) {
      alert("Game finished. Player 2 won.")
      startGame()
    }
    }
  
</script>


<template>
  <main>
    <form @submit.prevent="setParams" class="form__container">
      <div class="form__div">
        <h1>Set-up</h1>
        <div>
          <p>Dupla 1</p>
          <label for="d1j1">Jogador 1
            <input type="text" name="d1j1" id="d1j1" v-model="names.d1j1">

          </label>
          <br>
          <label for="d1j2">Jogador 2
            <input type="text" name="d1j2" id="d1j2" v-model="names.d1j2">

          </label>
        </div>
        <div>
          <p>Dupla 2</p>
          <label for="d2j1">Jogador 1
            <input type="text" name="d2j1" id="d2j1" v-model="names.d2j1">

          </label>
          <br>
          <label for="d2j2">Jogador 2
            <input type="text" name="d2j2" id="d2j2" v-model="names.d2j2">

          </label>
        </div>
          <br>
          <label for="maxSets">Número de Sets
            <input type="number" name="maxSets" id="maxSets" v-model="maxSets">

          </label>
          <br>
          <label for="maxGames">Número de games
            <input type="number" name="maxGames" id="maxGames" v-model="maxGames">

          </label>
          <br>
          <label for="maxTieBreak">Vitória em tie-break
            <input type="number" name="maxTieBreak" id="maxTieBreak" v-model="maxTieBreak">

          </label>
          <br>
          <input type="submit" value="Start Game" @click="startGame()">
      </div>
    </form>
    
      <div>
        <!-- More info on the functions on the <script> section. -->
        <table>
          <tr> <!-- First double scoreboard -->
            <td class="name">{{ names.d1j1.toUpperCase().substring(0,3) }} / {{ names.d1j2.toUpperCase().substring(0,3) }}</td>
            <td class="num">{{ set1 }}</td>
            <td class="num">{{ game1 }}</td>
            <td class="num">{{ score1 }}</td>
            <td class="num"><button class="score" @click="increaseScore1();">+</button></td>
          </tr>
          <tr> <!-- Second double scoreboard -->
            <td class="name">{{ names.d2j1.toUpperCase().substring(0,3) }} / {{ names.d2j2.toUpperCase().substring(0,3) }}</td>
            <td class="num">{{ set2 }}</td>
            <td class="num">{{ game2 }}</td>
            <td class="num">{{ score2 }}</td>
            <td class="num"><button class="score" @click="increaseScore2();">+</button></td>
          </tr>
        </table>
      </div>
  </main>
</template>

<style scoped> /* Just some placeholder styling for it to not look so simple. */
  main {
    font-family: Arial, Helvetica, sans-serif;
    zoom: 175%;
  }
  .score {
    width: 30px;
    height: 30px;
    background-color: rgb(138, 247, 138);
  }
  table {
    text-align: center;
    border: 1px black solid;
      
  }
  td {
    border: 1px black solid;
  }
  .num {
    height: 30px;
    width: 30px;
  }
  .name {
    width: 100px;
    height: 20px;
  }

  .form__container {  
    width: 500px;

  }

  .form__div {
    display: flex;
    flex-direction: column;
    
  }
</style>