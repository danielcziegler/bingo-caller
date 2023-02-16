<template>
  <div v-if="sequence !== undefined">
    <v-row class="mb-5">
      <v-col cols="6">
        <v-btn
          color="success"
          size="x-large"
          variant="tonal"
          stacked
          prepend-icon="mdi-arrow-right"
          @click="drawNumber()"
          :disabled="numbersRemaining === 0"
          >Draw Number</v-btn
        >
      </v-col>
      <v-col cols="6">
        <v-btn
          color="error"
          size="x-large"
          variant="tonal"
          stacked
          prepend-icon="mdi-exit-to-app"
          @click="$emit('newGame')"
          >New Game</v-btn
        >
      </v-col>
    </v-row>
    <v-divider class="mb-5"></v-divider>
    <div v-if="numbersDrawn > 0" class="text-h1">
      {{ drawnNumbers[0] }}
    </div>
    <div
      v-if="numbersDrawn > 0 && drawnNumbers[0] in numberNicknames"
      class="text-h6"
    >
      {{ numberNicknames[drawnNumbers[0]] }}
    </div>
    <v-divider class="mb-5 mt-5"></v-divider>
    <div>Numbers Drawn: {{ numbersDrawn }}</div>
    <div>Numbers Remaining: {{ numbersRemaining }}</div>
    <div v-if="numbersDrawn > 0">All Numbers Drawn: {{ drawnNumbers }}</div>
  </div>
</template>

<script>
export default {
  name: "Game",
  data() {
    return {
      sequence: undefined,
    };
  },
  props: {
    game: {
      type: String,
      required: true,
    },
  },
  computed: {
    numberNicknames: function () {
      return {
        1: "Kelly's Eye",
        17: "Dancing Queen",
        32: "Mehhhh",
        43: "Down on your Knees",
      };
    },
    numbersDrawn: function () {
      if (this.sequence === undefined) {
        return undefined;
      }
      return this.sequence[0];
    },
    numbersRemaining: function () {
      if (this.sequence === undefined) {
        return undefined;
      }
      return 90 - this.numbersDrawn;
    },
    drawnNumbers: function () {
      if (this.sequence === undefined) {
        return undefined;
      }
      let numbers = JSON.parse(JSON.stringify(this.sequence));
      numbers.shift();
      let drawn = [];
      if (this.numbersDrawn > 0) {
        let counter = 0;
        while (counter < this.numbersDrawn) {
          drawn.unshift(numbers[counter]);
          counter++;
        }
      }
      return drawn;
    },
  },
  methods: {
    generateSequenceFromHash(hash) {
      return atob(decodeURIComponent(hash));
    },
    drawNumber() {
      if (this.sequence === undefined) {
        return;
      }
      this.sequence[0] = this.numbersDrawn + 1;
    },
  },
  mounted() {
    this.sequence = JSON.parse(this.generateSequenceFromHash(this.game));
  },
};
</script>
