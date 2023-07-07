<template>
    <div class="weather-widget">
      <div class="location-input">
        <label for="location" class="location-label">Enter Location:</label>
        <input type="text" id="location" v-model="location" class="location-input-field" />
      </div>
      <button @click="fetchWeatherData" class="get-weather-button">Get Weather</button>
      <div v-if="weatherData" class="weather-data">
        <div class="weather-result">
          <p class="location">Location: {{ weatherData.name }}</p>
          <img v-if="weatherData.weather" :src="getWeatherIconUrl(weatherData.weather[0].icon)" :alt="weatherData.weather[0].description" class="weather-icon" />
          <p v-if="weatherData.weather" class="temperature">
            {{ weatherData.weather[0].description }}
          </p>
          <p v-if="weatherData.main" class="temperature">
            Temperature: {{ weatherData.main.temp.toFixed(1) }}°C
          </p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        location: '',
        weatherData: null
      };
    },
    methods: {
      async fetchWeatherData() {
        try {
          const apiKey = 'b7bfca7b27a3485144fea086c50d09dc';
          const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${apiKey}`;
  
          const response = await fetch(apiUrl);
          const data = await response.json();
  
          this.weatherData = data;
        } catch (error) {
          console.error('Error fetching weather data:', error);
        }
      },
      getWeatherIconUrl(iconCode) {
        return `https://openweathermap.org/img/w/${iconCode}.png`;
      }
    }
  };
  </script>
  
  <style scoped>
  .weather-widget {
    border: 1px solid #ccc;
    padding: 20px;
    margin-bottom: 20px;
    text-align: center;
    background: #f5f5f5;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
  
  .location-input {
    margin-bottom: 15px;
  }
  
  .location-label {
    margin-right: 10px;
    font-size: 18px;
    font-weight: bold;
    color: #333;
  }
  
  .location-input-field {
    padding: 5px 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-top: 5px;
  }
  
  .get-weather-button {
    padding: 8px 20px;
    font-size: 16px;
    font-weight: bold;
    color: #fff;
    background-color: #4caf50;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .get-weather-button:hover {
    background-color: #45a049;
  }
  
  .weather-data {
    margin-top: 10px;
  }
  
  .weather-result {
    border: 4px solid #ccc;
    padding: 10px;
    background: linear-gradient(to bottom, rgba(190, 228, 252, 0.5), rgba(30, 144, 255, 0.5));
  }
  
  .location {
    font-size: 18px;
    font-weight: bold;
    color: #333;
  }
  
  .temperature {
    font-size: 16px;
    color: #666;
  }
  
  .weather-icon {
    width: 60px;
    height: 60px;
  }
  
  .temperature {
    font-size: 24px;
    color: #ff5722;
  }
  </style>