<template>
    <div class="photo-widget">
      <h2 class="title">{{ title }}</h2>
      <div class="photo-container">
        <img :src="photoUrl" :alt="title" />
      </div>
      <button @click="getRandomPhoto" class="load-button">Load New Photo</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        title: '',
        photoUrl: '',
      };
    },
    mounted() {
      this.getRandomPhoto();
    },
    methods: {
      async getRandomPhoto() {
        try {
          const apiKey = '38037020-2c48722c03be8437a05b588e6';
          const apiUrl = `https://pixabay.com/api/?key=${apiKey}&q=nature&image_type=photo&orientation=horizontal`;
  
          const response = await fetch(apiUrl);
          const data = await response.json();
  
          const randomIndex = Math.floor(Math.random() * data.hits.length);
          const randomPhoto = data.hits[randomIndex];
  
          this.title = randomPhoto.tags;
          this.photoUrl = randomPhoto.webformatURL;
        } catch (error) {
          console.error('Error fetching random photo:', error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .photo-widget {
    border: 1px solid #ccc;
    padding: 20px;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .title {
    margin-bottom: 10px;
    font-size: 24px;
    color: white;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 4px;
  }
  
  .photo-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: fit-content;
    max-width: 100%;
    max-height: 300px;
    overflow: hidden;
    margin-top: 10px;
    border-radius: 4px;
    border: 5px solid #ccc;
  }
  
  .photo-container img {
    width: 100%;
    height: auto;
    object-fit: cover;
  }
  
  .load-button {
    margin-top: 10px;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    color: white;
    background-color: #4caf50;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .load-button:hover {
    background-color: #45a049;
  }
  </style>