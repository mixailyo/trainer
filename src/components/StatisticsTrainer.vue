<template>
  <div class="stats">
    <div><span>Скорость:</span> {{ speed }} зн./мин</div>
    <div><span>Точность:</span> {{ accuracy }}%</div>
    <template v-if="lastSpeed > 0">
      <div><span>Последняя скорость:</span> {{ lastSpeed }} зн./мин</div>
      <div><span>Последняя точность:</span> {{ lastAccuracy }}%</div>
    </template>
  </div>
</template>

<script>
export default {
  data() {
    return {
      speed: 0,
      accuracy: 0,
      errors: [],
      startTime: 0,
      lastSpeed: 0,
      lastAccuracy: 0,
    };
  },

  props: ["trainerStatus", "mainText", "inputText"],

  mounted() {
    if (localStorage.speed) {
      this.lastSpeed = localStorage.speed;
    }
    if (localStorage.accuracy) {
      this.lastAccuracy = localStorage.accuracy;
    }
  },

  methods: {
    counterSpeed() {
      let int = setInterval(() => {
        if (this.trainerStatus === 1) {
          let allTime = (Date.now() - this.startTime) / 60000;
          let availabilityErros = false;
          for (let i = 0; i < this.inputText.length; i++) {
            if (this.inputText[i] != this.mainText[i]) availabilityErros = true;
          }
          if (!availabilityErros) {
            this.speed = Math.round(this.inputText.length / allTime);
          }
        }

        // Stop interval
        if (this.trainerStatus === 2) clearInterval(int);
      }, 1000);
    },
    addToLocalStorage() {
      localStorage.speed = this.speed;
      this.lastSpeed = this.speed;
      localStorage.accuracy = this.accuracy;
      this.lastAccuracy = this.accuracy;
    },
    clearStats() {
      this.speed = 0;
      this.accuracy = 0;
      this.errors = [];
      this.startTime = 0;
    },
  },

  watch: {
    inputText: function () {
      // Start timer
      if (this.inputText.length == 1) {
        this.startTime = Date.now();
      }

      // Сount the accuracy
      if (
        this.mainText[this.inputText.length - 1] !=
          this.inputText[this.inputText.length - 1] &&
        this.errors.indexOf(this.inputText.length) == -1
      ) {
        this.errors.push(this.inputText.length);
      }
      if (this.trainerStatus === 1) {
        this.accuracy = (
          ((this.mainText.length - this.errors.length) / this.mainText.length) *
          100
        ).toFixed(2);
      }
    },
  },
};
</script>

<style lang="scss">
.stats {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  width: 100%;
  color: white;
  background-color: black;
  text-transform: uppercase;
  div {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid white;
    width: 50%;
    height: 100%;
    padding: 15px;
    span {
      font-weight: bold;
      margin-right: 5px;
    }
  }
}

@media screen and (max-width: 490px) {
  .stats {
    div {
      padding: 10px;
      font-size: 10px;
    }
  }
}
</style>