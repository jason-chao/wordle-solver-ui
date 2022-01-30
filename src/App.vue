<template>
  <div class="container">
    <div class="row justify-content-center mb-4">
      <div class="col-10 col-md-5">
        <p class="fs-1 fw-bolder mb-2">WORDLE SOLVER</p>
      </div>
      <div class="col-1">
        <button
          type="button"
          class="btn btn-lg btn-link fs-3"
          data-bs-toggle="modal"
          data-bs-target="#aboutModal"
        >
          <i class="bi bi-question-circle-fill text-secondary"></i>
        </button>
      </div>
    </div>
    <div class="row justify-content-center mb-5 text-secondary">
      <div class="col-6 col-md-3">
        <p>Number of letters</p>
      </div>
      <div class="col-5 col-md-3">
        <input
          type="range"
          class="form-range"
          min="4"
          max="11"
          step="1"
          v-model="wordLength"
          @change="wordLengthChanged"
        />
      </div>
      <div class="col-1 col-md-1">
        {{ wordLength }}
      </div>
      <div class="col-6 col-md-3 text-right">
        <div class="form-check">
          <input
            class="ms-0 form-check-input"
            type="checkbox"
            v-model="includePlurals"
            id="includePlurals"
          />
          <label class="form-check-label" for="includePlurals">
            include plurals
          </label>
        </div>
      </div>
    </div>
    <Solver
      :wordLength="wordLength"
      :includePlurals="includePlurals"
      ref="solver"
    ></Solver>
  </div>
  <div
    class="modal fade"
    id="aboutModal"
    tabindex="-1"
    aria-labelledby="aboutModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="aboutModalLabel">About WORDLE SOLVER</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body text-start">
          <p>
            Wordle-Solver helps people who struggle with vocabulary enjoy the
            famous game of
            <a href="https://www.powerlanguage.co.uk/wordle/" target="_blank"
              >WORDLE</a
            >. So that those who find WORDLE extremely hard, just like the
            developer of Wordle-Solver, will not be embarrassed for not being
            able to show their tries.
          </p>
          <p>
            Wordle-Solver is positioned as a solver for the WORDLE game in
            general. Wordle-Solver supports a variable word length and works
            with other implementations of WORDLE. Therefore, it relies on a
            broader word list and does not target that of the original
            implementation of WORDLE.
          </p>
          <p>
            Based on probability, Wordle-Solver helps players find the best
            words for the next try.
          </p>
          <p>Repeat the following steps until you find the hidden word:</p>
          <ul>
            <li>
              Tap the “Get suggested words” button to get a list of suggested
              words.
            </li>
            <li>
              Pick a word suggested by Wordle-Solver and enter it into WORDLE.
              (Or, if you wish to try your own guess, type it in the box below
              the suggested words and tap “Add”.)
            </li>
            <li>
              Change the colours of the letter tiles by tapping the tiles to
              match those shown by WORDLE.
            </li>
          </ul>
          <p>
            In the classical setting of five letters, Wordle-Solver takes an
            average of 3.7 tries to pinpoint the hidden word and solves 99.7% of
            words commonly used in English within the limit of 6 tries if they
            are chosen as hidden words. In most cases, after 3 - 4 tries,
            Wordle-Solver is able to pinpoint the hidden word.
          </p>
          <p>
            If interested, follow the developer
            <a href="https://twitter.com/jasontchao" target="_blank"
              ><i class="bi bi-twitter"></i> @jasontchao</a
            >
            and check out 
            <a
              href="https://github.com/jason-chao/wordle-solver"
              target="_blank"
              ><i class="bi bi-github"></i> Wordle-Solver's backend</a
            >.   Your feedback is greatly appreciated.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Solver from "./components/Solver.vue";

export default {
  name: "App",
  data: function () {
    return {
      wordLength: 5,
      includePlurals: false,
    };
  },
  components: {
    Solver,
  },
  methods: {
    wordLengthChanged: function () {
      this.$refs.solver.reset();
    },
  },
};
</script>

<style>
#app {
  font-family: "Courier New", Courier, monospace, Avenir, Helvetica, Arial,
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
