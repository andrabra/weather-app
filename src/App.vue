<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      error: null,
      info: null,
    };
  },
  computed: {
    cityName() {
      return `"${this.city}"`;
    },
    showTemp() {
      if (!this.info) {
        return '';
      }
      return `Температура: ${this.info.main.temp}`;
    },
    showHumidity() {
      return `Влажность: ${this.info.main.humidity}`;
    },
    showPressure() {
      return `Давление: ${this.info.main.pressure}`;
    },
    showWind() {
      return `Скорость ветра: ${this.info.wind.speed}`;
    },
    showFeelsLike() {
      return `Ощущается как: ${this.info.main.feels_like}`;
    },
    showMinTemp() {
      return `Минимальная температура: ${this.info.main.temp_min}`;
    },
    showMaxTemp() {
      return `Максимальная температура: ${this.info.main.temp_max}`;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Название должно содержать больше 2-х символов';
        return false;
      } else {
        this.error = null;
        axios
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ea9eab706e690b5cf17e970dc26d5b22`
          )
          .then((res) => (this.info = res.data))
          .catch((err) => (this.error = err.message));
      }
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погода</h1>
    <p class="description">
      Узнать погоду в <span v-if="!city">вашем городе</span>
      <span v-else
        >городе <span class="bold">{{ cityName }}</span>
      </span>
    </p>

    <!-- <div class="actions-wrapper"> -->
    <form @submit.prevent="getWeather" class="actions-wrapper">
      <input v-model="city" type="text" placeholder="Введите город..." />
      <button v-if="city" type="submit">Получить погоду</button>
    </form>
    <!-- </div> -->
    <p v-if="error" class="error">{{ error }}</p>

    <div v-if="info" class="info-wrapper">
      <p>{{ showTemp }}</p>
      <p>{{ showHumidity }}</p>
      <p>{{ showPressure }}</p>
      <p>{{ showWind }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  padding: 20px;
  border-radius: 50px;
  background: #16114a;
  color: #fff;
  text-align: center;

  display: flex;
  flex-direction: column;
  align-items: center;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper .description {
  margin-top: 20px;
}

.wrapper input {
  background: transparent;
  border: none;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  transition: all 0.2s ease;
}

.wrapper input:focus {
  border-bottom-color: var(--primary-color);
}

.wrapper button {
  border: none;
  border-radius: 10px;
  background: var(--primary-color);
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-3px);
}

.wrapper .actions-wrapper {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 15px;
  max-width: 400px;
}

.wrapper .info-wrapper {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  gap: 5px;
}
</style>
