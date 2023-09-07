<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <table>
    <tr>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[0] @playerMoved="handlePlayerMoved(0, $event)" @gameOver="handleGameOver(0)"/>
      </td>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[1] @playerMoved="handlePlayerMoved(1, $event)" @gameOver="handleGameOver(1)"/>
      </td>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[2] @playerMoved="handlePlayerMoved(2, $event)" @gameOver="handleGameOver(2)"/>
      </td>
    </tr>
    <tr>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[3] @playerMoved="handlePlayerMoved(3, $event)" @gameOver="handleGameOver(3)"/>
      </td>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[4] @playerMoved="handlePlayerMoved(4, $event)" @gameOver="handleGameOver(4)"/>
      </td>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[5] @playerMoved="handlePlayerMoved(5, $event)" @gameOver="handleGameOver(5)"/>
      </td>
    </tr>
    <tr>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[6] @playerMoved="handlePlayerMoved(6, $event)" @gameOver="handleGameOver(6)"/>
      </td>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[7] @playerMoved="handlePlayerMoved(7, $event)" @gameOver="handleGameOver(7)"/>
      </td>
      <td>
        <TicTacToe :currentPlayer=this.currentPlayer :boardIsActive=boardActiveStatuses[8] @playerMoved="handlePlayerMoved(8, $event)" @gameOver="handleGameOver(8)"/>
      </td>
    </tr>
  </table>
</template>

<script>
import TicTacToe from './components/TicTacToe.vue'

export default {
  name: 'App',
  components: {
    TicTacToe
  },
  data: function() {
    return {
      currentPlayer: 0,
      boardActiveStatuses: [true, true, true, true, true, true, true, true, true],
      boardWinners: [-1, -1, -1, -1, -1, -1, -1, -1, -1]
    }
  },
  methods: {
    handlePlayerMoved: function(boardNum, {moveIndex, didWinBoard}) {
      if (didWinBoard) {
        this.boardWinners[boardNum] = this.currentPlayer;
        this.checkWinner()
      }
      if (this.boardWinners[moveIndex] == -1) {
        console.log('moveIndex = ' + moveIndex)
        for (let i=0; i<9; i++) {
          this.boardActiveStatuses[i] = false;
        }
        this.boardActiveStatuses[moveIndex] = true;
      }
      else {
        for (let i=0; i<9; i++) {
          this.boardActiveStatuses[i] = true;
        }
      }
      this.currentPlayer = (this.currentPlayer === 0) ? 1 : 0;
    },
    checkWinner: function() {
      let lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];
      for (var i = 0; i < lines.length; i++) {
          if (this.boardWinners[lines[i][0]] != -1 &&
            this.boardWinners[lines[i][0]] != -2 &&
            this.boardWinners[lines[i][0]] == this.boardWinners[lines[i][1]] &&
            this.boardWinners[lines[i][1]] == this.boardWinners[lines[i][2]]) {
            console.log('Winning row = ' + lines[i]);
            return {winningBoardNums: lines[i].slice(), winningPlayer: this.boardWinners[lines[i][0]]}
          }
      }
      return {winningBoardNums: null, winningPlayer: null};
    },

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
