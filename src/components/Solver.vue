<template>
  <div class="container">
    <WordList
      :wordLength="wordLength"
      @onSearchParametersChanged="onSearchParametersChanged"
      ref="wordListComponent"
    ></WordList>
    <div class="row justify-content-center mt-4 mb-4">
      <div class="col-10">
        <button
          type="button"
          class="btn btn-primary btn-lg"
          :class="isLoading || gotHiddenWord ? 'disabled' : ''"
          @click="getSuggestedWords"
          v-if="suggestedWords.length <= 0"
        >
          <i class="bi bi-grid-3x3-gap-fill" v-if="showDoneChangingColours"></i>
          <span v-if="showDoneChangingColours"> Done setting the colours</span>
          <i class="bi bi-arrow-right-short" v-if="showDoneChangingColours"></i>
          <i class="bi bi-binoculars"></i> Get suggested words
        </button>
      </div>
    </div>
    <div
      v-for="suggestedWord in suggestedWords"
      v-bind:key="suggestedWord"
      class="row justify-content-center mt-2 mb-4"
    >
      <div class="col-5">
        <p class="fs-1 mt-1 mb-1 fw-bolder" style="letter-spacing: 1rem">
          {{ suggestedWord }}
        </p>
      </div>
      <div class="col-1">
        <button
          type="button"
          class="btn btn-link btn-lg mt-1 fs-3"
          @click="addSuggestedWord(suggestedWord)"
        >
          <i class="bi bi-arrow-left-circle-fill"></i>
        </button>
      </div>
    </div>
    <div class="row justify-content-center mt-2 mb-4" v-if="isLoading">
      <div class="col-2">
        <div
          class="spinner-border text-primary"
          style="width: 3rem; height: 3rem"
          role="status"
        >
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>
    </div>
    <div class="row justify-content-center mt-2 mb-3" v-if="showAlert">
      <div class="col-10 col-md-6">
        <div class="alert alert-primary" role="alert">
          {{ alertMessage }}
        </div>
      </div>
    </div>
    <div class="row justify-content-center mt-4 mb-5">
      <div class="col-6">
        <button
          type="button"
          class="btn btn-success btn-lg"
          @click="reset"
          v-if="wordSearchParameters.length > 0"
        >
          <i class="bi bi-arrow-clockwise"></i> Reset
        </button>
      </div>
    </div>
    <WordInput
      :wordLength="wordLength"
      @onWordAdded="addWordManually"
    ></WordInput>
  </div>
</template>

<script>
import WordList from "./WordList.vue";
import WordInput from "./WordInput.vue";
import axios from "axios";

export default {
  name: "App",
  computed: {
    showAlert: function () {
      if (this.alertMessage.length <= 0) return false;
      return true;
    },
    showDoneChangingColours: function () {
      if (this.wordSearchParameters.length <= 0) return false;
      return true;
    },
  },
  data: function () {
    return {
      suggestedWords: [],
      suggestedWordsSource: "",
      wordSearchParameters: [],
      alertMessage:
        "Tap 🡰 to select a suggested word. If you wish to try your word, please type it below.  Tap a tile to change its colour.",
      isLoading: false,
      gotHiddenWord: false,
    };
  },
  components: {
    WordList,
    WordInput,
  },
  methods: {
    getSuggestedWords: function () {
      if (this.wordSearchParameters.length > 0) {
        let allCorrect = /^\++$/;
        let lastWordSymbols =
          this.wordSearchParameters[this.wordSearchParameters.length - 1]
            .symbols;
        if (allCorrect.test(lastWordSymbols)) {
          this.alertMessage = "Great! You got the hidden word.";
          this.gotHiddenWord = true;
          return;
        }
      }
      this.isLoading = true;
      axios
        .post(
          process.env.VUE_APP_WORDLE_SOLVER_API_URL +
            "?wordlength=" +
            this.wordLength.toString() +
            "&plurals=" +
            (this.includePlurals ? "true" : "false"),
          this.wordSearchParameters
        )
        .then((response) => {
          if (response.status == 200) {
            let suggstedWords = response.data.suggested_words;
            suggstedWords = suggstedWords.map(function (w) {
              return w.toUpperCase();
            });
            this.suggestedWords = suggstedWords;
            this.suggestedWordsSource = response.data.word_list;
            if (this.suggestedWords.length <= 0) {
              this.alertMessage =
                "Sorry, there are no other possible words.  Please check the colours of letter tiles.";
            }
          }
        })
        .catch(() => {
          this.alertMessage =
            "Sorry, the server is unreachable.  Please check your internet connection.";
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
    addSuggestedWord: function (newWord) {
      this.alertMessage = "";
      this.$refs.wordListComponent.addWord(newWord);
      this.suggestedWords.length = 0;
    },
    addWordManually: function (newWord) {
      this.alertMessage = "";
      this.$refs.wordListComponent.addWord(newWord);
    },
    onSearchParametersChanged: function (args) {
      this.alertMessage = "";
      this.wordSearchParameters = args;
    },
    reset: function () {
      this.suggestedWords = [];
      this.suggestedWordsSource = "";
      this.wordSearchParameters = [];
      this.alertMessage = "";
      this.gotHiddenWord = false;
      this.$refs.wordListComponent.reset();
    },
  },
  props: {
    wordLength: Number,
    includePlurals: Boolean,
  },
};
</script>