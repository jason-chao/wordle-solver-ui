<template>
  <div class="row justify-content-center">
    <div class="col-10 col-md-6">
      <div class="input-group input-group-lg">
        <span class="input-group-text">Your own guess</span>
        <input
          type="text"
          class="form-control"
          :maxlength="wordLength"
          v-model="wordInput"
        />
        <button type="button" class="btn btn-primary btn-lg" @click="addWord">
          Add
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      wordInput: "",
    };
  },
  emits: ["onWordAdded"],
  methods: {
    addWord: function () {
      this.wordInput = this.wordInput.toUpperCase();
      this.$emit("onWordAdded", this.wordInput);
      this.wordInput = "";
    },
  },
  props: {
    wordLength: Number,
  },
};
</script>
