<template>
  <header>
    <h1>Grab A Beer</h1>
    <p v-if="gameInProgress">Time left: {{ time }} seconds</p>
  </header>
  <main ref="main">
    <div
      v-if="gameInProgress"
      class="beer-icon"
      @click="grabBeer()"
      :style="{ gridColumnStart: column, gridRowStart: row }"
    ></div>
    <div class="menu" v-if="!gameInProgress">
      <button class="start-game" @click="startNewGame()">Start Game</button>
      <p class="score" v-if="displayScore">
        You grabbed {{ score }} beers - Cheers!
      </p>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      score: 0,
      time: 0,
      column: 0,
      row: 0,
      moveTimer: 0,
      displayScore: false,
    };
  },
  methods: {
    startNewGame() {
      this.displayScore = false;
      this.score = 0;
      this.time = 10;

      this.beerPosition();
      this.moveBeerRandom(1000);

      const interval = setInterval(() => {
        this.time--;
        if (this.time === 0) {
          clearInterval(interval);
          clearInterval(this.moveTimer);
          this.displayScore = true;
        }
      }, 1000);
    },
    generateRandomNumber(min, max) {
      return Math.round(Math.random() * (max - min) + min);
    },
    beerPosition() {
      this.column = this.generateRandomNumber(1, this.gridColumns);
      this.row = this.generateRandomNumber(1, this.gridRows);
    },
    moveBeerRandom(interval) {
      this.moveTimer = setInterval(() => {
        this.beerPosition();
      }, interval);
    },
    grabBeer() {
      if (this.time === 0) {
        return;
      }

      this.score++;

      clearInterval(this.moveTimer);
      this.beerPosition();
      this.moveBeerRandom(1000);
    },
  },
  computed: {
    gameInProgress() {
      return this.time !== 0;
    },
    gridColumns() {
      const gridEl = this.$refs.main;
      const styles = window.getComputedStyle(gridEl);
      return styles.gridTemplateColumns.split(" ").length;
    },
    gridRows() {
      const gridEl = this.$refs.main;
      const styles = window.getComputedStyle(gridEl);
      return styles.gridTemplateRows.split(" ").length;
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  font-size: 2rem;
}

header > * {
  padding: 0;
  margin: 0;
}

main {
  position: relative;
  background-color: aqua;

  display: grid;
  grid-template-columns: repeat(auto-fill, 8rem);
  grid-template-rows: repeat(auto-fill, 8rem);
  justify-content: center;
  align-items: center;
}

.menu {
  position: absolute;
  justify-self: center;
  align-self: center;

  display: flex;
  flex-direction: column;
  align-items: center;
}

.start-game {
  all: unset;
  display: inline-block;

  font-size: 2rem;
  font-family: monospace;

  border: 2px solid black;
  border-radius: 5%;

  padding: 1rem 0.5rem;

  background-color: snow;

  cursor: pointer;
}

.beer-icon {
  display: block;
  background-image: url("../assets/beer.svg");
  background-size: cover;

  width: 7rem;
  padding: 0.5rem;
  aspect-ratio: 1/1;
}

.score {
  font-size: 4rem;
  font-weight: 600;

  text-align: center;
}
</style>
