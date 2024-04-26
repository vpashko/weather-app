<script lang="ts">
  import axios from 'axios';

  export default {
    data () {
      return {
        city: '',
        error: '',
        info: null
      }
    },
    computed: {
      cityName () {
        return '"' + this.city + '"';
      }
    },
    methods: {
      getWeather () {
        if (this.city.trim().length < 2) {
          this.error = 'City name is too short';
          return false;
        }

        this.error = '';

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=5fe5494e56a8905910f89c0f0e1ab9b5`)
          .then(res => this.info = res.data)
      }
    }
  }
</script>

<template>
  <div class="wrapper">
    <h1 class="title">Weather App</h1>
    <p class="message">Find out the weather in {{ city === '' ? 'your city' : cityName }}</p>

    <input type="text" placeholder="Enter the city" class="cityInput" v-model="city">
    <button class="seacrhButton" v-if="city != ''" @click="getWeather()">Search</button>
    <button disabled class="seacrhButton" v-else>Type something</button>

    <p class="error" v-show="error != ''">{{ error }}</p>

    <div v-if="info" class="info">
      <p class="stats">Description: {{ (info as any)?.weather[0].description }}</p>
      <p class="stats">Temperature: {{ (info as any)?.main?.temp }}°C</p>
      <p class="stats">Feels like: {{ (info as any)?.main?.feels_like }}°C</p>
      <p class="stats">Humidity: {{ (info as any)?.main?.humidity }}%</p>
      <p class="stats">Pressure: {{ (info as any)?.main?.pressure }}hPa</p>
      <p class="stats">Wind speed: {{ (info as any)?.wind?.speed }}m/s</p>
    </div>
  </div>
</template>

<style scoped>

  .error {
    color: #d03939;
    margin-top: 10px;
  }
  .wrapper {
    width: 900px;
    height: 500px;
    padding: 20px;
    border-radius: 50px;
    background: #1f0f24;
    text-align: center;
    color: #fff;
  }

  .title {
    font-size: 40px;
    margin-top: 50px;
  }

  .message {
    font-size: 20px;
    margin-top: 20px;
  }

  .cityInput {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    transition: all 0.3s ease-in-out;
  }

  .cityInput:focus {
    border-bottom-color: #6e2d7d ;
  }

  .seacrhButton {
    background: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }

  .seacrhButton:hover {
    transform: scale(1.1);
  }

  .seacrhButton:disabled {
    background: #746027;
    cursor: not-allowed;
  }

  .info {
    margin-top: 30px;
    font-size: 20px;
  }

  .stats {
    margin-top: 10px;
  }
</style>
