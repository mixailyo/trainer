<template>
  <div id="app">
    <div class="container">
      <h1>Научись быстро печатать с клавиатурным тренажером</h1>
      <main-text
        ref="mainText"
        @getMainText="getMainText"
        :trainerStatus="trainerStatus"
        :inputText="inputText"
      />
      <statistics-trainer
        ref="statisticTrainer"
        class="statistics-trainer"
        :trainerStatus="trainerStatus"
        :mainText="mainText"
        :inputText="inputText"
      />
      <input-text
        ref="inputText"
        :trainerStatus="trainerStatus"
        @getInputText="getInputText"
      ></input-text>
      <button-trainer @click="clickButton">{{ buttonText }}</button-trainer>
    </div>
  </div>
</template>

<script>
import MainText from "@/components/MainText.vue";
import StatisticsTrainer from "@/components/StatisticsTrainer.vue";
import InputText from "@/components/InputText.vue";
import ButtonTrainer from "@/components/UI/ButtonTrainer.vue";

export default {
  name: "App",

  components: { MainText, StatisticsTrainer, InputText, ButtonTrainer },

  data() {
    return {
      buttonText: "Начать печатать",
      trainerStatus: 0,
      inputText: "",
    };
  },

  methods: {
    clickButton() {
      this.trainerStatus === 0 ? this.startTrainer() : this.newTrainer();
    },

    async startTrainer() {
      // Change button text
      this.buttonText = "Начать заново";

      // Get the text
      await this.$refs.mainText.getText();

      // Change trainer status
      this.trainerStatus = 1;

      // Count speed
      this.$refs.statisticTrainer.counterSpeed();
    },

    getMainText(mainText) {
      this.mainText = mainText;
    },

    getInputText(inputText) {
      this.inputText = inputText;
    },

    newTrainer() {
      // Add last statistic to localStorage
      this.$refs.statisticTrainer.addToLocalStorage();

      // Reset data
      this.buttonText = "Начать печатать";
      this.trainerStatus = 0;
      this.mainText = ["Текст скрыт до старта тренажера"];
      this.inputText = "";
      this.currentChar = 0;

      // Clear main text
      this.$refs.mainText.clearText();

      // Clear statistic
      this.$refs.statisticTrainer.clearStats();

      // Clear input text
      this.$refs.inputText.clearInputText();
    },
  },

  watch: {
    inputText: function () {
      // End trainer
      if (this.mainText.join("") === this.inputText) {
        this.trainerStatus = 2;
        this.inputText = "";
        this.$refs.mainText.endTrainer();
        this.$refs.inputText.clearInputText();
      }
    },
  },
};
</script>

<style lang="scss">
// Reset
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

button {
  padding: 0;
  border: none;
  font: inherit;
  color: inherit;
  background-color: transparent;
  cursor: pointer;
}

// Main styles
body {
  font-family: "Roboto", sans-serif;
  font-size: 16px;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 980px;
  margin: 0 auto;
  padding: 30px 15px;
}

h1 {
  text-align: center;
  font-size: 1.9em;
  margin-bottom: 30px;
  text-transform: uppercase;
}

.main-text {
  margin-bottom: 30px;
}

.statistics-trainer {
  margin-bottom: 30px;
}

.input-text {
  margin-bottom: 30px;
}

// Media styles
@media screen and (max-width: 490px) {
  body {
    font-size: 12px;
  }

  h1 {
    font-size: 1.46em;
  }
}
</style>
