<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

let currentPlayer;
let playerSymbols = new Map();
playerSymbols.set(1,"&#10006;");
playerSymbols.set(2,"&#9898;");
let game = new Array(3).fill([0,0,0]);

function initiateGame() {
  document.getElementById("gameBoard").style.display = "flex";
  for (let i = 0; i < 3;i++){
    game[i] = new Array(3).fill(0);
  }
  let gameBoxes = document.getElementsByClassName("gameBox");
  for(let gameBox of gameBoxes){
    gameBox.innerHTML = "";
  }
  currentPlayer = 1;
}

function checkIfWinner(currentRow,currentColumn) {
  let hasWon = false;
  //Only check diagonals if field is on a diagonal. 
  // because the id is 0 to 8 diagonal fields will always equal %2 === 0
  if((currentRow + currentColumn) % 2 === 0) {
    console.log("diagonalField");
    hasWon = checkDiagonals(currentRow, currentColumn);
  }
  //Only check if there is no win on the diagonal.
  if(!hasWon) {
    hasWon = checkRowsAndColumns(currentRow, currentColumn);
  }
  return hasWon;
}

function checkDiagonals(currentRow, currentColumn) {
  let hasWon = true;
  //diagonal from top left to bottom right
    for (let i = 0; i< game.length-1; i++) {
      if(currentRow === currentColumn) {
        if(game[i][i] !== game[i+1][i+1]){
          hasWon = false;
          break;
        }
      }
      // diagonal from bottom left to top right.
      //currentRow === 1 to check both diagonals in case the middle box was selected
      if(currentRow === 1 || currentRow !== currentColumn){
        if(game[game.length - i - 1][i] !== game[game.length - i - 2][i + 1]) {
          hasWon = false;
          break;
        }
      }
  }
  console.log("diagonal win", hasWon);
  return hasWon;
}

function checkRowsAndColumns(currentRow, currentColumn) {
  let hasWon = true;
  for(let i = 0; i < game.length-1; i++) {
      if(game[currentRow][i] !== game[currentRow][i + 1]) {
        hasWon = false;
        break;
      }
    }

    //Only check columns if there is no win in rows
  if(!hasWon) {
    hasWon = true;
    for(let i = 0; i < game.length-1; i++) {
      if(game[i][currentColumn] !== game[i + 1][currentColumn]) {
        hasWon = false;
        break;
      }
    }
  }
  return hasWon;
}

function setBoxValue(event) {
    let clickedBox = event.target;
    let currentRow = Math.floor(parseInt(clickedBox.id) / 3);
    let currentColumn = parseInt(clickedBox.id) % 3;
    if( game[currentRow][currentColumn] !== 0) {
      return;
    }
    game[currentRow][currentColumn] = currentPlayer;
    console.log("nulstellenl", game.filter((entry) => entry.filter(number => number === 0)).length);
    if(!game.includes(0)){
      console.log("draw");
    }
    
    if(checkIfWinner(currentRow, currentColumn)){
      clickedBox.innerHTML = "WW";
      document.getElementById("gameBoard").style.display = "none";
      return;
    }
    clickedBox.innerHTML = playerSymbols.get(currentPlayer);
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
    <div id="gameBoard">
      <div class="gameBox" id="0" @click="setBoxValue"></div>
      <div class="gameBox" id="1" @click="setBoxValue"></div>
      <div class="gameBox" id="2" @click="setBoxValue"></div>
      <div class="gameBox" id="3" @click="setBoxValue"></div>
      <div class="gameBox" id="4" @click="setBoxValue"></div>
      <div class="gameBox" id="5" @click="setBoxValue"></div>
      <div class="gameBox" id="6" @click="setBoxValue"></div>
      <div class="gameBox" id="7" @click="setBoxValue"></div>
      <div class="gameBox" id="8" @click="setBoxValue"></div>
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

#gameBoard {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
}



</style>
