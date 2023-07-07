<template>
    <div class="location-widget">
      <div v-if="latitude && longitude" class="current-location">
        <p>Latitude: {{ latitude }}</p>
        <p>Longitude: {{ longitude }}</p>
      </div>
  
      <div class="location-input">
        <label for="latitude">Latitude:</label>
        <input type="text" id="latitude" v-model="inputLatitude" />
      </div>
      <div class="location-input">
        <label for="longitude">Longitude:</label>
        <input type="text" id="longitude" v-model="inputLongitude" />
      </div>
  
      <button @click="fetchLocationDetails" class="get-location-details-button">Find Location Details</button>
  
      <div v-if="foundLocation" class="location-details">
        <h3>Location Details</h3>
        <div class="location-info">
          <p><strong>Country:</strong> {{ foundLocation.components.country }}</p>
          <p><strong>Postal Code:</strong> {{ foundLocation.components.postcode }}</p>
          <p><strong>Place:</strong> {{ getCityName() }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        latitude: null,
        longitude: null,
        inputLatitude: '',
        inputLongitude: '',
        foundLocation: null,
      };
    },
    mounted() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.getPosition);
      }
    },
    methods: {
      getPosition(position) {
        this.latitude = position.coords.latitude;
        this.longitude = position.coords.longitude;
      },
      async fetchLocationDetails() {
        try {
          const apiKey = '92591005a7b94008909d59a64b6d2a49';
          const latitude = this.inputLatitude || this.latitude;
          const longitude = this.inputLongitude || this.longitude;
          const apiUrl = `https://api.opencagedata.com/geocode/v1/json?q=${encodeURIComponent(
            latitude + ',' + longitude
          )}&key=${apiKey}`;
  
          const response = await fetch(apiUrl);
          const data = await response.json();
  
          if (data.results && data.results.length > 0) {
            const location = data.results[0];
            this.foundLocation = location;
            console.log('Location:', location);
          }
        } catch (error) {
          console.error('Error fetching location data:', error);
        }
      },
      getCityName() {
        if (this.foundLocation && this.foundLocation.components) {
          return this.foundLocation.components.city || this.foundLocation.components.town || this.foundLocation.components.village;
        }
        return '';
      },
    },
  };
  </script>
  
  <style scoped>
  .location-widget {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border: 1px solid #ccc;
    padding: 20px;
    margin-bottom: 20px;
    text-align: center;
    background-color: #f5f5f5;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .current-location {
    margin-bottom: 10px;
  }
  
  .location-input {
    display: grid;
    grid-template-columns: max-content 1fr;
    gap: 10px;
    align-items: center;
    margin-bottom: 15px;
  }
  
  .location-widget p {
    color: #666;
    margin-bottom: 5px;
  }
  
  .location-input label {
    width: 100px;
    color: #333;
    font-size: 16px;
    font-weight: bold;
  }
  
  .location-input input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
  }
  
  .get-location-details-button {
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
  
  .get-location-details-button:hover {
    background-color: #45a049;
  }
  .location-details {
    margin-top: 20px;
    text-align: left;
    padding: 10px;
    background-color: #e7e7e7;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }
  
  .location-details h3 {
    margin-bottom: 10px;
    color: #333;
    font-size: 18px;
  }
  
  .location-details .location-info {
    color: #fff;
    margin-bottom: 5px;
  }
  
  .error-message {
    color: red;
    margin-top: 10px;
  }
  
  .location-widget > *:not(:last-child) {
    margin-bottom: 10px;
  }
  
  .location-details .location-info:first-child {
    margin-top: 10px;
  }
  </style>