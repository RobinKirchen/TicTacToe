<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

let currentPlayer = 1;
let playerOneSymbol = "&#10006;"
let game = new Array(3);

function initiateGame() {
  for (let i = 0; i < 3;i++){
    game[i] = [0, 0, 0];
  }
}

function checkIfWinner() {
  //check Rows
  //check Columns
  //check diagonals

}
function setBoxValue(event) {
    let clickedBox = event.target;
    game[Math.floor(parseInt(clickedBox.id)/3)][parseInt(clickedBox.id)%3] = currentPlayer;
    console.log("field",parseInt(clickedBox.id)/3);
    clickedBox.innerHTML = game[Math.floor(parseInt(clickedBox.id)/3)][parseInt(clickedBox.id)%3];
    console.log(game);
    currentPlayer = (currentPlayer %2) + 1;
}

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <div class="gameBoard">
      <div class="gameBox" id="0" @click="setBoxValue"></div>
      <div class="gameBox" id="1" @click="setBoxValue">test</div>
      <div class="gameBox" id="2" @click="setBoxValue">test</div>
      <div class="gameBox" id="3" @click="setBoxValue">test</div>
      <div class="gameBox" id="4" @click="setBoxValue">test</div>
      <div class="gameBox" id="5" @click="setBoxValue">test</div>
      <div class="gameBox" id="6" @click="setBoxValue">test</div>
      <div class="gameBox" id="7" @click="setBoxValue">test</div>
      <div class="gameBox" id="8" @click="setBoxValue">test</div>
    </div>
    <button @click="initiateGame">Spiel starten</button>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

.gameBox {
  font-size: 50px;
  width: 100px;
  height: 100px;
  border: 1px solid black;
  text-align: center;
}

.gameBoard {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
}



</style>
