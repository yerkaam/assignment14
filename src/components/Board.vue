<template>
  <div class="game-board">
    <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
      <div v-for="(cell, colIndex) in row" :key="colIndex" class="cell" @click="makeMove(rowIndex, colIndex)">
        <span class="symbol">{{ cell }}</span>
      </div>
    </div>
    <div class="button" @click="clearCells()">Reset</div>
     <div v-if="gameOver" class="game-over-message">
      <p v-if="winner">Player {{ winner }} wins!</p>
      <p v-else>It's a draw!</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
      currentPlayer: 'X',
      gameOver: false,
      winner: null
    };
  },
  methods: {
    makeMove(row, col) {
        if(!this.gameOver && this.board[row][col] ===''){
            this.board[row][col] = this.currentPlayer;
            if(this.checkWin(row,col)){
                this.gameOver = true;
                this.winner = this.currentPlayer;
            }else if(this.checkDraw()){
                this.gameOver = true;
            }else{
                this.switchPlayer();
            }
        }
    },
    checkWin(row,col){
        if (
        this.board[row][0] === this.currentPlayer &&
        this.board[row][1] === this.currentPlayer &&
        this.board[row][2] === this.currentPlayer
      ) {
        return true;
      }

    
      if (
        this.board[0][col] === this.currentPlayer &&
        this.board[1][col] === this.currentPlayer &&
        this.board[2][col] === this.currentPlayer
      ) {
        return true;
      }

  
      if (
        (row === col &&
          this.board[0][0] === this.currentPlayer &&
          this.board[1][1] === this.currentPlayer &&
          this.board[2][2] === this.currentPlayer) ||
        (row + col === 2 &&
          this.board[0][2] === this.currentPlayer &&
          this.board[1][1] === this.currentPlayer &&
          this.board[2][0] === this.currentPlayer)
      ) {
        return true;
      }

      return false;
    },
    checkDraw(){
          for (let row = 0; row < 3; row++) {
        for (let col = 0; col < 3; col++) {
          if (this.board[row][col] === '') {
            return false; 
          }
        }
      }
      return true;  
    },
     switchPlayer() {
      this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
    },
    clearCells(){
           for (let row = 0; row < 3; row++) {
            for (let col = 0; col < 3; col++) {
            this.board[row][col] = '';
            this.gameOver = false;
        }
      }
        }
  }
};
</script>

<style scoped>
.game-board {
  width: fit-content ; 
  margin: 100px auto ; 
}

.row {
  display: flex ; 
}

.cell {
  width: 100px ; 
  height: 100px ; 
  border: 2px solid ; 
  font-size: 60px;
    text-align: center;
  align-items: center ;

 
}
.symbol{
     display: flex;
    justify-content: center;
    align-items: center;
    color: red;
    height: 100%;
}
.button{
    margin: 20px auto;
    width: 30%;
    border-radius: 5px;
    color:#d9e1cc;
    background-color: rgb(112, 69, 69) ;
    text-align: center;
    padding: 5px 10px;
}
.button:hover{
 opacity: 0.5;
}
</style>

