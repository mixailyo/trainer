<template>
  <textarea
    @input="getInputText()"
    v-model="inputText"
    type="text"
    class="input-text"
    placeholder="Набирайте здесь текст после старта тренажера"
    :disabled="trainerStatus != 1"
  />
</template>

<script>
export default {
  data() {
    return {
      inputText: "",
    };
  },

  props: ["trainerStatus"],

  methods: {
    getInputText() {
      this.$emit("getInputText", this.inputText);
    },

    clearInputText() {
      this.inputText = "";
    },
  },
};
</script>

<style lang="scss">
.input-text {
  width: 100%;
  min-height: 200px;
  padding: 15px;
  font-family: inherit;
  font-size: inherit;
}
</style>