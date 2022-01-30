<template>
  <div class="row justify-content-center mb-3" :class="hidden ? 'd-none' : ''">
    <div class="col-0 col-md-2 col-lg-3"></div>
    <div class="col pe-0 ps-0" v-for="i in wordLengthArray" :key="i">
      <LetterTile
        :letter="word[i]"
        :letterIndex="i"
        @onLetterStateChanged="letterStateChanged"
      ></LetterTile>
    </div>
    <div class="col pe-0">
      <div class="mt-1">
        <button
          v-if="!confirmed"
          type="button"
          class="btn btn-lg btn-link fs-3"
          @click="removeWord"
        >
          <i class="bi bi-x-circle-fill text-danger"></i>
        </button>
      </div>
    </div>
    <div class="col-0 col-md-2 col-lg-3"></div>
  </div>
</template>


<script>
import LetterTile from "./LetterTile.vue";

export default {
  name: "WordRow",
  data: function () {
    return {
      wordState: "",
      wordLengthArray: [],
      confirmed: false,
    };
  },
  emits: ["onWordRemoved", "onWordStateChanged"],
  components: {
    LetterTile,
  },
  methods: {
    removeWord: function () {
      if (confirm(`Are you sure you want to remove '${this.word}'?`) == true) {
        this.$emit("onWordRemoved", {
          word: this.word,
          index: this.wordIndex,
        });
      }
    },
    letterStateChanged: function (args) {
      let letterStates = this.wordState.split("");
      letterStates[args.letterIndex] = args.state;
      this.wordState = letterStates.join("");
      this.$emit("onWordStateChanged", {
        word: this.word,
        state: this.wordState,
        index: this.wordIndex,
      });
    },
    initialiseDefaultValues: function () {
      let defaultWordState = "";
      let defaultWordLengthArray = [];
      for (let i = 0; i < this.wordLength; i++) {
        defaultWordState += "_";
        defaultWordLengthArray.push(i);
      }
      this.wordState = defaultWordState;
      this.wordLengthArray = defaultWordLengthArray;
    },
  },
  props: {
    word: String,
    wordLength: Number,
    wordIndex: Number,
    hidden: Boolean,
  },
  created: function () {
    this.initialiseDefaultValues();
  },
};
</script>
