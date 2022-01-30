<template>
  <LetterRow
    v-for="i in wordIndexDisplayArray"
    :key="i"
    :word="words[i]"
    @onWordStateChanged="wordStateChanged"
    :wordLength="wordLength"
    :wordIndex="i"
    :hidden="wordRemovedArray[i]"
    @onWordRemoved="wordRemoved"
  ></LetterRow>
</template>

<script>
import LetterRow from "./LetterRow.vue";

export default {
  name: "WordList",
  computed: {
    wordIndexDisplayArray: function () {
      return this.wordIndexArray;
    },
  },
  data: function () {
    return {
      words: [],
      wordStates: [],
      wordIndexArray: [],
      wordRemovedArray: [],
      searchParameters: [],
    };
  },
  emits: ["onSearchParametersChanged"],
  components: {
    LetterRow,
  },
  methods: {
    updateSearchParameters: function () {
      this.searchParameters.length = 0;
      for (let i = 0; i < this.words.length; i++) {
        if (!this.wordRemovedArray[i]) {
          this.searchParameters.push({
            word: this.words[i],
            symbols: this.wordStates[i],
          });
        }
      }
      this.$emit("onSearchParametersChanged", this.searchParameters);
    },
    wordStateChanged: function (args) {
      if (this.words[args.index] == args.word) {
        this.wordStates[args.index] = args.state;
        this.updateSearchParameters();
      }
      /* console.log(args.index + " - " + args.word + ":" + args.state); */
    },
    wordRemoved: function (args) {
      if (this.words[args.index] == args.word) {
        this.wordRemovedArray[args.index] = true;
        this.updateSearchParameters();
      }
    },
    addWord: function (newWord) {
      console.log("got " + newWord);
      if (newWord.length == this.wordLength) {
        this.words.push(newWord);
        let defaultWordState = "";
        for (let i = 0; i < this.wordLength; i++) defaultWordState += "_";
        this.wordStates.push(defaultWordState);
        this.wordIndexArray.push(this.words.length - 1);
        this.wordRemovedArray.push(false);
        console.log(newWord);
        this.updateSearchParameters();
      }
    },
    reset: function () {
      this.wordIndexArray = [];
      this.wordRemovedArray = [];
      this.searchParameters = [];
      this.words = [];
      this.wordStates = [];
    },
  },
  props: {
    wordLength: Number,
  },
};
</script>
