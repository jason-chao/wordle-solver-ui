<template>
    <button class="btn btn-primary btn-block btn-lg pb-0"
    :class="displayBackground"
    @click="changeState"
    >
     <p class="h1 fw-2 pt-1 pb-0 ps-1 pe-1">{{ letter }}</p>
    </button>
</template>

<script>
export default {
  computed: {
    displayBackground: function () {
      if (this.state == "+") return "btn-success";
      else if (this.state == "?") return "btn-warning";
      else return "btn-secondary";
    },
  },
  data: function () {
    return {
      state: "_",
    };
  },
  emits:["onLetterStateChanged"],
  methods: {
    changeState: function () {
      switch (this.state) {
        case "+":
          this.state = "_";
          break;
        case "?":
          this.state = "+";
          break;
        default:
          this.state = "?";
      }
      this.$emit("onLetterStateChanged", { state: this.state, letterIndex: this.letterIndex });
    },
  },
  props: {
    letter: String,
    letterIndex: Number,
  },
};
</script>