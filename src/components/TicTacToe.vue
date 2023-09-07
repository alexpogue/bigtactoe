<template>
  <div id="tictactoe">
    <div v-if="winningChar != null">
      Winner: {{ winningChar }}
    </div>
    <div v-else>
      Current turn: {{ playerPieces[currentPlayer] }}
    </div>
    <table style="border-collapse: collapse" :class="{ 'grayedOut': !boardIsActive }">
      <tr v-for="(n, y) in 3" :key="n">
        <td v-for="(item, x) in board.slice(y * 3, y * 3 + 3)" @click="markSquare(y, x)" v-bind:class="{'winningSquare':(board[y * 3 + x].winning === true)}" :key="item.id"><div class="content">{{ item.text }}</div></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TicTacToe',
  props: {
    currentPlayer: Number,
    boardIsActive: Boolean
  },
  data: function() {
    return {
      board: [
        {text: ' ', winning: false, id: 0}, {text: ' ', winning: false, id: 1}, {text: ' ', winning: false, id: 2},
        {text: ' ', winning: false, id: 3}, {text: ' ', winning: false, id: 4}, {text: ' ', winning: false, id: 5},
        {text: ' ', winning: false, id: 6}, {text: ' ', winning: false, id: 7}, {text: ' ', winning: false, id: 8}
      ],
      winningChar: null,
      winningSquares:null,
      playerPieces: ['X', 'O'],
    }
  },
  methods: {
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
          if (this.board[lines[i][0]].text != ' ' &&
            this.board[lines[i][0]].text == this.board[lines[i][1]].text &&
            this.board[lines[i][1]].text == this.board[lines[i][2]].text) {
            console.log('Winning row = ' + lines[i]);
            return {winningSquares: lines[i].slice(), winningChar: this.board[lines[i][0]].text}
          }
      }
      return {winningSquares: null, winningChar: null};
    },
    setWinningSquares: function(winningSquares) {
      for (var i = 0; i < winningSquares.length; i++) {
        this.board[winningSquares[i]].winning = true;
      }
    },
    markSquare: function(y, x) {
      if (!this.boardIsActive || this.winningChar != null) {
        return
      }
      var indexToMark = y * 3 + x;
      var curVal = this.board[indexToMark].text;
      if (curVal != 'X' && curVal != 'O') {
        console.log('currentPlayer = ' + this.currentPlayer);
        var currentChar = this.playerPieces[this.currentPlayer];
        //Vue.set(this.board, indexToMark, {text: currentChar, winning: false})
        this.board[indexToMark].text = currentChar;
        this.board[indexToMark].winning = false;
        //let nextPlayer = (this.currentPlayer === 0) ? 1 : 0;
        //this.currentPlayer = (this.currentPlayer === 0) ? 1 : 0;

        var winnerResponse = this.checkWinner();
        this.winningChar = winnerResponse.winningChar;
        this.winningSquares = winnerResponse.winningSquares;
        let didWin = false;
        if (this.winningSquares !== null) {
          didWin = true
          
          this.setWinningSquares(this.winningSquares);
          console.log("board = " + JSON.stringify(this.board));
        }
        
        this.$emit('playerMoved', {moveIndex: indexToMark, didWinBoard: didWin})
      }
    }
  }
}
</script>

<style scoped>
table, th, td {
    border: 1px solid #000;
    border-collapse: collapse;
}               
                
table {         
    table-layout: fixed;
    width: 200px;
    height: 200px;  
}                   
                
td, th {    
    width: 33.33%;
    position: relative;
    text-align: center;
}
td:after, th:after {
    content: '';
    display: block;
    margin-top: 100%;
}

td .content {
    position: absolute;
    top: 25px;
    bottom: 0;
    left: 0;
    right: 0;
}

.winningSquare {
    background-color: #FF0;
}

.grayedOut {
    background-color: #999;
}
</style>
