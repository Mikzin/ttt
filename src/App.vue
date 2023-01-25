<template>
  <div id="app">
    <div class="grid">
      <div
        class="cell"
        v-for="(cell, index) in cells"
        :key="index"
        @click="play(index)"
      >
        {{ cell }}
      </div>
    </div>
    <div class="turn-info">
      <p>{{ currentPlayer }}'s turn</p>
    </div>
    <button @click="restart">Restart</button>
    <!-- Add a modal to display the result -->
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <div class="modal-header">
          <h2>Result</h2>
        </div>
        <div class="modal-body">
          <p v-if="winner">{{ winner }} wins!</p>
          <p v-else-if="isDraw">It's a draw!</p>
        </div>
        <div class="modal-footer">
          <button @click="closeModal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cells: Array(9).fill(null),
      xIsNext: true,
      winner: null,
      showModal: false,
    };
  },
  computed: {
    isDraw() {
      return !this.winner && !this.cells.includes(null);
    },
    currentPlayer() {
      return this.xIsNext ? 'Player 1(X)' : 'Player 2(O)';
    },
  },
  methods: {
    play(index) {
      if (this.winner || this.cells[index]) {
        return;
      }
      this.cells.splice(index, 1, this.xIsNext ? 'X' : 'O');
      this.xIsNext = !this.xIsNext;
      this.checkWin();
    },
    checkWin() {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          this.cells[a] &&
          this.cells[a] === this.cells[b] &&
          this.cells[a] === this.cells[c]
        ) {
          this.winner = this.cells[a];
          this.showModal = true;
          break;
        }
      }
      if (this.isDraw) {
        this.showModal = true;
      }
    },
    closeModal() {
      this.showModal = false;
    },
    restart() {
      this.cells.fill(null);
      this.winner = null;
      this.showModal = false;
    },
  },
};
</script>

<style>
/* Apply some basic styles */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, Helvetica, sans-serif;
  background-color: #f7f7f7;
  padding: 20px;
}
/* Style the grid */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  margin-bottom: 20px;
}
/* Style the cells */
.cell {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  background-color: #fff;
  color: #000;
  border: 2px solid #ddd;
  cursor: pointer;
}
/* Style the result display */
.result {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
}
/* Style the restart button */
button {
  padding: 8px 16px;
  background-color: #0084ff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  text-transform: uppercase;
}
button:hover {
  background-color: #0073e6;
}
</style>
