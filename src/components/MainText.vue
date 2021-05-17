<template>
  <div :class="{ end: trainerStatus === 2 }" class="main-text">
    <span
      :class="{
        currentChar: trainerStatus === 1 && i == inputText.length,
        correct: trainerStatus === 1 && i == inputText.length,
        wrong:
          trainerStatus && mainText[i] != inputText[i] && i < inputText.length,
      }"
      v-for="(char, i) in mainText"
      :key="i"
      >{{ char }}</span
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      mainText: ["Текст скрыт до старта тренажера"],
    };
  },

  props: ["trainerStatus", "inputText"],

  methods: {
    async getText() {
      let url =
        "https://baconipsum.com/api/?type=meat-and-filler&paras=1&format=text";
      let response = await fetch(url);
      let responseText = await response.text();
      this.mainText = responseText
        .split("")
        .filter((char, i, arr) => !(char === " " && arr[i + 1] === " "));
      this.$emit("getMainText", this.mainText);
    },

    clearText() {
      this.mainText = ["Текст скрыт до старта тренажера"];
    },

    endTrainer() {
      this.mainText = [
        `Прекрасно! Ты прошел тренажер. Ниже можешь видеть свои показатели скорости и точности! Если хочешь еще раз пройти тренажер нажми на кнопку "Начать заново".`,
      ];
    },
  },
};
</script>

<style lang="scss">
.main-text {
  width: 100%;
  padding: 15px;
  border: 2px solid black;
  .currentChar {
    padding: 3px;
    border-radius: 3px;
  }
  .correct {
    background-color: black;
    color: white;
  }
  .wrong {
    background-color: red;
  }
  &.end {
    background-color: black;
    color: white;
  }
}
</style>