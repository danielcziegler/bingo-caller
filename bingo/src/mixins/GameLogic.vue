<script>
export default {
  data() {},
  methods: {
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
    generateSequenceFromHash(hash) {
      return atob(decodeURIComponent(hash));
    },
  },
};
</script>
