<script setup>

let currentPlayer;
let players = [
  {
    id: 1,
    symbol: "&#10006;",
    score: 0
  },
  {
    id: 2,
    symbol: "&#9898;",
    score: 0
  }
];
let game = new Array(3).fill([0,0,0]);

function initiateGame() {
  document.getElementById("gameBoard").style.display = "flex";
  for (let i = 0; i < 3; i++){
    game[i] = new Array(3).fill(0);
  }
  let gameBoxes = document.getElementsByClassName("gameBox");
  for(let gameBox of gameBoxes){
    gameBox.innerHTML = "";
  }
  currentPlayer = players.find(player => player.id === 1);
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

function checkIfGameOver(currentRow, currentColumn) {
  let gameInfo = "";
  let isOver = false;
  if(checkIfWinner(currentRow, currentColumn)){
      currentPlayer.score++;
      gameInfo = `Scores: ${players[0].score} : ${players[1].score}`;
      isOver = true;
    }

    //if no 0 value remains all fields have been set
    if(game.filter(entry => entry.includes(0)).length < 1){
      console.log("Game", game.includes(0));
      isOver = true;
      gameInfo = "draw";
    }

    if(isOver){
      document.getElementById("gameBoard").style.display = "none";
      document.getElementById("scores").innerHTML = gameInfo;
    }
}

function setBoxValue(event) {
    let clickedBox = event.target;
    let currentRow = Math.floor(parseInt(clickedBox.id) / 3);
    let currentColumn = parseInt(clickedBox.id) % 3;
    if( game[currentRow][currentColumn] !== 0) {
      return;
    }
    game[currentRow][currentColumn] = currentPlayer.id;
    
    checkIfGameOver(currentRow, currentColumn);

    clickedBox.innerHTML = currentPlayer.symbol;
    console.log(game);
    currentPlayer = players.find(player => player.id === (currentPlayer.id % players.length) + 1);
}

</script>

<template>

  <main>
    <div id="scores"></div>
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
.gameBox {
  font-size: 50px;
  width: 100px;
  height: 100px;
  border: 1px solid black;
  text-align: center;
  align-items: center;
}

#gameBoard {
  display: none;
  flex-wrap: wrap;
  width: 300px;
  text-align: center;
}

#scores {
  font-size: 50px;
  display: flex;
  width: 300px;
  text-align: center;
}

</style>
