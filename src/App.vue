<script setup>
  import {onMounted, ref, watch} from "vue";
  // Creation of all the variables needed. Unfortunately, i don´t know yet how to optimize this :(
  const score1 = ref(0)
  const score2 = ref(0)
  const game1 = ref(0)
  const game2 = ref(0)
  const set1 = ref(0)
  const set2 = ref(0)
  const maxGames = ref(6)
  let tieBreak = false
  let isFinished
  
  onMounted(() => {
    startGame()
  })
  
  
  // Declarations of the methods used for most of the program functionalities.

  // Function responsible for resetting the settings on the game when a new game is started.
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

  // Function responsible for checking if a game is tied at 5x5. If it is, automatically adjusts for the game to end on 7x5
  function testForTie() {
    if (game1.value == 5 && game2.value == 5) {
      console.log("The game hit a soft tie and must end by a 2 game advantage or tie-break on 6-6")
      maxGames.value = 7
    }
  }

  // Function responsible for increasing each player's points.
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

    switch (game1.value) {
      case maxGames.value:
        set1.value++
        game1.value = 0
        game2.value = 0
        console.log("Set increased")
        break;

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

    switch (game2.value) {
      case maxGames.value:
        set2.value++
        game1.value = 0
        game2.value = 0
        console.log("Set increased")
        break;

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
      <!-- More info on the functions on the <script> section. -->
      <button @click="startGame()">Start game</button>
      <table>
        <tr> <!-- First double scoreboard -->
          <td class="name">COE/TAP</td>
          <td class="num">{{ set1 }}</td>
          <td class="num">{{ game1 }}</td>
          <td class="num">{{ score1 }}</td>
          <td class="num"><button class="score" @click="increaseScore1(); testForTie()">+</button></td>
        </tr>
        <tr> <!-- Second double scoreboard -->
          <td class="name">DIN/STU</td>
          <td class="num">{{ set2 }}</td>
          <td class="num">{{ game2 }}</td>
          <td class="num">{{ score2 }}</td>
          <td class="num"><button class="score" @click="increaseScore2(); testForTie()">+</button></td>
        </tr>
      </table>
    </div>
  </main>
</template>

<style scoped> /* Just some provisory styling for it to not look so simple. */
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