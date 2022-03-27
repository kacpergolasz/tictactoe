<script>
import Field from "./Field.vue";

export default {
  props: {
    size: {
      type: Number,
      default: 3,
    },
  },
  components: {
    Field,
  },

  data() {
    return {
      currentPlayer: 0,
      movesCount: 0,
      battlefield: [],
    };
  },

  computed: {
    movesToDo() {
      return Math.pow(this.size, 2);
    },
  },

  created() {
    const arrayOfSize = Array.from(Array(this.size).keys());
    this.battlefield = arrayOfSize.map(() => arrayOfSize.map(() => null));
  },

  methods: {
    changePlayer() {
      this.currentPlayer = +!this.currentPlayer;
    },

    doTheMove(col, row) {
      this.battlefield[row][col] = this.currentPlayer;

      this.checkWinner(col, row);

      this.movesCount++;
      if (this.movesCount === this.movesToDo) {
        this.tie();
      }

      this.changePlayer();
    },

    checkWinner(col, row) {
      const winInRow = this.battlefield[row];
      if (this.checkIfWon(winInRow)) {
        this.playerWon();
      }

      const winInCol = this.battlefield.map((r) => r[col]);
      if (this.checkIfWon(winInCol)) {
        this.playerWon();
      }

      if (col === row) {
        const fieldsInMainDiagonal = this.battlefield.map(
          (r, index) => r[index]
        );
        if (this.checkIfWon(fieldsInMainDiagonal)) {
          this.playerWon();
        }
      }

      if (col === this.size - 1 - row) {
        const fieldsInOppositeDiagonal = this.battlefield.map(
          (r, index) => r[this.size - 1 - index]
        );
        if (this.checkIfWon(fieldsInOppositeDiagonal)) {
          this.playerWon();
        }
      }
    },

    checkIfWon(array) {
      return array.every((value) => value === this.currentPlayer);
    },

    tie() {
      this.$emit("tie");
    },

    playerWon() {
      this.$emit("win", this.currentPlayer);
      return null
    },

  },
};
</script>

<template>
  <div class="game">
    <h1>Player {{ currentPlayer + 1 }}</h1>
    <div class="game__row" v-for="row in size" :key="row">
      <Field
        v-for="column in size"
        :key="column + '' + row"
        :column="column - 1"
        :row="row - 1"
        :value="battlefield[row - 1][column - 1]"
        @change.once="(col, row) => doTheMove(col, row)"
      />
    </div>
  </div>
</template>
