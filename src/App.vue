<script>
import GameBoard from "./components/GameBoard.vue";
export default {
  data() {
    return {
      gameIsOver: false,
      winner: null,
      showGame: true,
    };
  },
  components: {
    GameBoard,
  },

  methods: {
    tie() {
      this.gameIsOver = true;
    },

    playerWon(player) {
      this.winner = player;
      this.gameIsOver = true;
    },

    restartGame() {
      this.showGame = false;
      setTimeout(() => {
        this.showGame = true;
        this.gameIsOver = false;
        this.winner = null;
      }, 1);
    },
  },
};
</script>

<template>
  <div>
    <GameBoard
      v-if="showGame"
      :size="3"
      @tie="tie()"
      @win="(player) => playerWon(player)"
    />

    <div v-if="gameIsOver">
      <template v-if="winner === null"> {{ "It's a tie!" }} </template>
      <template v-else> Player {{ winner + 1 }} won! </template>
      <button @click="restartGame()">Restart</button>
    </div>
  </div>
</template>

<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
