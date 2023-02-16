<template>
  <v-app>
    <!-- <v-app-bar color="grey-lighten-4" :elevation="0" border>
      <v-app-bar-title>NBCC Bingo</v-app-bar-title>
    </v-app-bar> -->
    <v-main>
      <v-container class="fill-height">
        <v-responsive class="d-flex align-center text-center fill-height">
          <div v-if="game === undefined">
            <v-btn
              color="success"
              size="x-large"
              variant="tonal"
              stacked
              prepend-icon="mdi-plus-circle"
              @click="newGame()"
              >Start a New Game</v-btn
            >
          </div>
          <div v-else>
            <Game :game="game" @new-game="newGame()" />
          </div>
        </v-responsive>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Game from "@/components/Game.vue";
export default {
  name: "Bingo",
  data() {
    return {
      game: undefined,
    };
  },
  components: {
    Game,
  },
  methods: {
    newGame() {
      this.game = undefined;
      this.game = this.generateNewGameId();
    },
    generateNewGameId() {
      // create array of all possible numbers
      let sequence = [];
      let counter = 1;
      while (counter <= 90) {
        sequence.push(counter);
        counter++;
      }
      // randomize the sequence
      sequence = this.shuffleArray(sequence);
      // add 0 as first item (to indicate number of numbers drawn in game)
      sequence.unshift(0);
      // generate hash code of the sequence (gameid)
      const gameid = this.generateStringHash(JSON.stringify(sequence));
      return gameid;
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    generateStringHash(str) {
      return encodeURIComponent(btoa(str));
    },
  },
};
</script>
