<script setup>
  import axios from 'axios';
</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <div class="input-wrapper">
      <input v-model="city" type="text" placeholder="Enter your city" size="15" oninput="this.size=Math.max(this.value.length, 15)" onfocus="this.placeholder=''" onblur="this.placeholder='Enter your city'">
      <button v-if="city.length == 0" disabled @click="getWeather">Get the weather</button>
      <button v-else @click="getWeather">Get the weather</button>
    </div>
    <p class="error">{{ inputError }}</p>
    <p v-if="city.length == 0">City not selected</p>
    <p v-else>Weather in {{ cityName }}</p>
    <div v-if="info != null">
      <p>Condition: {{ showMain }}</p>
      <p>Temperature: {{ showTemp }}</p>
      <p>Feels like: {{ showFeelsLike }}</p>
      <p>Minimal Temperature: {{ showMinTemp }}</p>
      <p>Maximum Temperature: {{ showMaxTemp }}</p>
    </div>
    <p class="error">{{ infoError }}</p>
  </div>
</template>

<script>

export default {
  data() {
    return {
      city: '',
      inputError: '',
      info: null,
      infoError: '',
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Name is too short!';
        return;
      }
      this.error = '';
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8065e85f64ddab52a679d6e19111bfe6`)
      .then(
        value => {
          this.info = value;
          this.infoError = '';
      },
        error => {
          this.infoError = error; 
          this.info = '';
        })
    },
  },
  computed: {
    cityName() {
      return `${this.city[0].toUpperCase()}${this.city.slice(1)}`;
    },
    showTemp() {
      return this.info.data.main.temp;
    },
    showFeelsLike() {
      return this.info.data.main.feels_like;
    },
    showMinTemp() {
      return this.info.data.main.temp_min;
    },
    showMaxTemp() {
      return this.info.data.main.temp_max;
    },
    showMain() {
      return this.info.data.weather[0].main;
    },
  }
}
</script>

<style scoped>
  * {
    font-family: 'GolosUI', sans-serif;
  }
  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #1f0f24;
    text-align: center;
    color: #fff;
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
  }
  h1 {
    font-family: 'GolosUI';
    font-weight: 900;
    margin-top: 50px;
    font-size: 2em;
  }
  p {
    font-size: 1em;
    font-weight: 500;
  }
  .input-wrapper {
    width: 100%;
    display: flex;
    gap: 20px;
    justify-content: center;
    align-items: center;
  }
  input {
    min-height: 28px;
    padding: 0;
    border: none;
    border-bottom: 2px solid #1f0f24;
    background: #1f0f24;
    color: #fff;
    font-weight: 700;
    font-size: 1em;
    text-align: center;
  }
  input:focus {
      outline: none;
      height: 38px;
      border-bottom: 2px solid #fff;
  }
  button {
    padding: 10px;
    min-height: 50px;
    background: rgba(253,187,45,0.9);
    border: none;
    font-weight: 700;
    font-size: 1em;
    cursor: pointer;
  }
  button:hover {
    background: rgba(253,187,45,1);
  }
  button:disabled {
    background: rgba(187,187,187,1);
    color: #000;
  }
  .error {
    color: #d03939;
  }
</style>