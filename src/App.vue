<script setup>
  import {ref} from "vue";
  // Creation of all the variables needed. unfortunately, i don´t know yet how to optimize this :(
  const score1 = ref(0)
  const score2 = ref(0)
  const game1 = ref(0)
  const game2 = ref(0)
  const set1 = ref(0)
  const set2 = ref(0)
  
  // Creation of the functions responsible for increasing the scores of each player
  function increaseScore1() {
    if (score1.value == 0) {
      score1.value = 15
      console.log("Score increased")
    } else if (score1.value == 15) {
      score1.value = 30
      console.log("Score increased")
    } else if (score1.value == 30) {
      score1.value = 40
      console.log("Score increased")
    } else {
      score1.value = 0
      score2.value = 0
      game1.value++
      console.log("Score and game increased") // When a game is won, both the points must be returned to 0.
    }
    if (game1.value == 6) {
      game1.value = 0
      game2.value = 0
      set1.value++
      console.log("Game and Set increased") // Same as the points, both must be set to 0 again.
    }
  }

  // Pretty much a copypaste from the above function. Anyway, if I figure out how to optimize it, I'm doin it.
  function increaseScore2() {
    if (score2.value == 0) {
      score2.value = 15
      console.log("Score increased")
    } else if (score2.value == 15) {
      score2.value = 30
      console.log("Score increased")
    } else if (score2.value == 30) {
      score2.value = 40
      console.log("Score increased")
    } else {
      score2.value = 0
      score1.value = 0
      game2.value++
      console.log("Score and game increased") // When a game is won, both the points must be returned to 0.
    }
    if (game2.value == 6) {
      game2.value = 0
      game1.value = 0
      set2.value++
      console.log("Game and Set increased") // Same as the points, both must be set to 0 again.
    }
  }

</script>


<template>
  <main>
    <div>
      <table>
        <tr> <!-- First double scoreboard -->
          <td>COE/TAP</td>
          <td>{{ set1 }}</td>
          <td>{{ game1 }}</td>
          <td>{{ score1 }}</td>
          <td><button @click="increaseScore1()">+</button></td>
        </tr>
        <tr> <!-- Second double scoreboard -->
          <td>DIN/STU</td>
          <td>{{ set2 }}</td>
          <td>{{ game2 }}</td>
          <td>{{ score2 }}</td>
          <td><button @click="increaseScore2()">+</button></td>
        </tr>
      </table>
    </div>
  </main>
</template>