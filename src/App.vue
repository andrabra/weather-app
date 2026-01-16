<script setup>
import { ref, computed } from 'vue';
import axios from 'axios';

// Реактивные переменные
const city = ref('');
const error = ref(null);
const info = ref(null);

// Вычисляемые свойства
const cityName = computed(() => `"${city.value}"`);

const showTemp = computed(() => {
  if (!info.value) return '';
  return `Температура: ${info.value.main.temp}°C`;
});

const showHumidity = computed(() => {
  if (!info.value) return '';
  return `Влажность: ${info.value.main.humidity}%`;
});

const showPressure = computed(() => {
  if (!info.value) return '';
  return `Давление: ${info.value.main.pressure} hPa`;
});

const showWind = computed(() => {
  if (!info.value) return '';
  return `Скорость ветра: ${info.value.wind.speed} м/с`;
});

const showFeelsLike = computed(() => {
  if (!info.value) return '';
  return `Ощущается как: ${info.value.main.feels_like}°C`;
});

const showMinTemp = computed(() => {
  if (!info.value) return '';
  return `Минимальная температура: ${info.value.main.temp_min}°C`;
});

const showMaxTemp = computed(() => {
  if (!info.value) return '';
  return `Максимальная температура: ${info.value.main.temp_max}°C`;
});

// Методы
const getWeather = async () => {
  if (city.value.trim().length < 2) {
    error.value = 'Название должно содержать больше 2-х символов';
    return false;
  }

  error.value = null;

  try {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=ea9eab706e690b5cf17e970dc26d5b22&lang=ru`
    );
    info.value = response.data;
  } catch (err) {
    if (err.response?.data.cod === '404') {
      error.value = 'Город не найден. Проверьте название.';
    } else {
      error.value = err.message;
    }
  }
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

    <form @submit.prevent="getWeather" class="actions-wrapper">
      <input v-model="city" type="text" placeholder="Введите город..." />
      <button v-if="city" type="submit">Получить погоду</button>
    </form>
    <p v-if="error" class="error m-t-20">{{ error }}</p>

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
