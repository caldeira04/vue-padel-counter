<script setup>
  import {onMounted, ref, watch} from "vue";
  // Creation of all the variables needed. unfortunately, i don´t know yet how to optimize this :(
  const score1 = ref(0)
  const score2 = ref(0)
  const game1 = ref(0)
  const game2 = ref(0)
  const set1 = ref(0)
  const set2 = ref(0)
  const maxGames = ref(6)
  let tieBreak = false
  let isFinished


  function startGame() {
    console.clear()
    console.log("Game started")
    maxGames.value = 6
    score1.value = 0
    score2.value = 0
    game1.value = 0
    game2.value = 0
    set1.value = 0
    set2.value = 0
    isFinished = false

  }

  onMounted(() => {
    startGame()
    if (game1.value == 5 && game2.value == 5){
      console.log("Now, the game must go into 7-5 or tie-break!")
      maxGames.value == 7
    }
    if (game1.value == 6 && game2.value == 6) {
      console.log("Tie-break started!")
      tieBreak = true
    }
    if (tieBreak) {
      score1.value++
    }
  })


  // Creation of the functions responsible for increasing the scores of each player
  function increaseScore1() {
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
        score1.value = 0
        score2.value = 0
        game1.value++
        console.log("Game increased")
        break;
    }

    if (game1.value == maxGames.value) {
      game1.value = 0
      game2.value = 0
      set1.value++
      console.log("Game and Set increased") // Same as the points, both must be set to 0 again.
    }
    if (set1.value == 2) {
      isFinished = true
      console.log("Game finished. Player 1 won.")
    }
  }
  
  // Pretty much a copypaste from the above function. Anyway, if I figure out how to optimize it, I'm doin it.
  function increaseScore2() {
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
        score1.value = 0
        score2.value = 0
        game2.value++
        console.log("Game increased")
        break;
    }

    if (game2.value == maxGames.value) {
      game2.value = 0
      game1.value = 0
      set2.value++
      console.log("Game and Set increased") // Same as the points, both must be set to 0 again.
    }
    if (set2.value == 2) {
      isFinished = true
      console.log("Game finished. Player 2 won.")
    }
  }
</script>


<template>
  <main>
    <div>
      <button @click="startGame()">Start game</button>
      <table>
        <tr> <!-- First double scoreboard -->
          <td class="name">COE/TAP</td>
          <td class="num">{{ set1 }}</td>
          <td class="num">{{ game1 }}</td>
          <td class="num">{{ score1 }}</td>
          <td class="num"><button class="score" @click="increaseScore1()">+</button></td>
        </tr>
        <tr> <!-- Second double scoreboard -->
          <td class="name">DIN/STU</td>
          <td class="num">{{ set2 }}</td>
          <td class="num">{{ game2 }}</td>
          <td class="num">{{ score2 }}</td>
          <td class="num"><button class="score" @click="increaseScore2()">+</button></td>
        </tr>
      </table>
    </div>
  </main>
</template>

<style scoped>
  main {
    font-family: monospace;
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
</style>