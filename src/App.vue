<script>
export default {
  data() {
    return {
      city: '',
      error: null,
    };
  },
  computed: {
    cityName() {
      return `"${this.city}"`;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Название должно содержать больше 2-х символов';
        return false;
      } else {
        this.error = null;
      }
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погода</h1>
    <p>
      Узнать погоду в <span v-if="!city">вашем городе</span>
      <span v-else
        >городе <span class="bold">{{ cityName }}</span>
      </span>
    </p>

    <div class="actions-wrapper">
      <input v-model="city" type="text" placeholder="Введите город..." />
      <button v-if="city" @click="getWeather()">Получить погоду</button>
    </div>
    <p v-if="error" class="error">{{ error }}</p>
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

.wrapper p {
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
</style>
