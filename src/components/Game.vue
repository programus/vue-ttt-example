<template>
  <div class="game">
    <div class="game-board">
      <Board :squares="history[stepNumber].squares" @put="handleClick"></Board>
    </div>
    <div class="game-info">
      <div>{{ status }}</div>
      <ol>
        <li v-for="step of moves" :key="step.move">
          <button @click="jumpTo(step.move)">{{ step.desc }}</button>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
import Board from "./Board";

export default {
  name: "Game",
  components: {
    Board,
  },
  props: {},
  data() {
    return {
      history: [
        {
          squares: Array(9).fill(null)
        }
      ],
      stepNumber: 0,
      xIsNext: true
    };
  },
  mounted() {},
  computed: {
    currentSquares() {
      return this.history[this.stepNumber].squares;
    },
    winner() {
      return this.calculateWinner(this.currentSquares);
    },
    status() {
      return this.winner
        ? `Winner: ${this.winner}`
        : `Next player: ${this.xIsNext ? "X" : "O"}`;
    },
    moves() {
      return this.history.map((step, move) => {
        const desc = move ? `Go to move #${move}` : "Go to game start";
        return {
          move: move,
          desc: desc
        };
      });
    }
  },
  methods: {
    handleClick(i) {
      const hist = this.history.slice(0, this.stepNumber + 1);
      const current = hist[hist.length - 1];
      const squares = current.squares.slice();
      if (this.calculateWinner(squares) || squares[i]) {
        return;
      }
      squares[i] = this.xIsNext ? "X" : "O";
      this.history = hist.concat([
        {
          squares: squares
        }
      ]);
      this.stepNumber = hist.length;
      this.xIsNext = !this.xIsNext;
    },
    jumpTo(move) {
      this.stepNumber = move;
      this.xIsNext = move % 2 === 0;
    },
    calculateWinner(squares) {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];
      for (let line of lines) {
        const [a, b, c] = line;
        if (
          squares[a] &&
          squares[a] === squares[b] &&
          squares[a] === squares[c]
        ) {
          return squares[a];
        }
      }
      return null;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
