<template>
    <div id="app">
      <h1 class="title">Word Scramble</h1>
      <div class="category-select">
        <label for="category">Select Category:</label>
        <select id="category" v-model="selectedCategory" @change="changeCategory">
          <option v-for="category in wordCategories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>
      </div>
      <p v-if="!gameOver" class="instruction">Unscramble the word:</p>
      <div v-if="!gameOver" class="word">
        <span
          class="word-letter"
          v-for="(letter, index) in scrambledWord"
          :key="index"
          @click="addLetter(letter)"
        >
          {{ letter }}
        </span>
      </div>
      <div class="user-input">
        <div
          class="letter"
          v-for="(letter, index) in userInput"
          :key="index"
        >
          {{ letter }}
        </div>
      </div>
      <button :disabled="gameOver" @click="checkAnswer">Check</button>
      <p v-if="gameOver" class="result-message">{{ resultMessage }}</p>
      <button v-if="gameOver" @click="restartGame">Play Again</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        wordCategories: [
          "Furniture",
          "Fruits",
          "Cities",
          "Animals",
          "Countries",
          "Colors",
          "Vegetables",
          "Sports",
          "Professions",
          "Transportation"
        ],
        words: {
          Furniture: ["table", "chair", "book", "pencil", "lamp"],
          Fruits: ["apple", "orange", "mango", "banana", "grape"],
          Cities: ["jakarta", "bandung", "surabaya", "yogyakarta", "medan"],
          Animals: ["cat", "dog", "elephant", "lion", "horse"],
          Countries: ["indonesia", "america", "japan", "england", "canada"],
          Colors: ["red", "blue", "green", "yellow", "purple"],
          Vegetables: ["carrot", "broccoli", "tomato", "cucumber", "spinach"],
          Sports: ["soccer", "basketball", "tennis", "swimming", "golf"],
          Professions: ["doctor", "teacher", "engineer", "lawyer", "chef"],
          Transportation: ["car", "train", "bicycle", "bus", "airplane"]
        },
        selectedCategory: "Furniture",
        scrambledWord: "",
        answer: "",
        userInput: [],
        resultMessage: "",
        gameOver: false
      };
    },
    mounted() {
      this.generateWord();
    },
    methods: {
      generateWord() {
        const categoryWords = this.words[this.selectedCategory];
        const randomIndex = Math.floor(Math.random() * categoryWords.length);
        const word = categoryWords[randomIndex];
        this.answer = word;
        this.scrambledWord = this.shuffleWord(word);
      },
      shuffleWord(word) {
        let shuffledWord = "";
        const wordArray = word.split("");
        while (wordArray.length > 0) {
          const randomIndex = Math.floor(Math.random() * wordArray.length);
          shuffledWord += wordArray[randomIndex];
          wordArray.splice(randomIndex, 1);
        }
        return shuffledWord;
      },
      addLetter(letter) {
        this.userInput.push(letter);
      },
      checkAnswer() {
        const userInputString = this.userInput.join("").toLowerCase();
        if (userInputString === this.answer.toLowerCase()) {
          this.resultMessage =
          "Congratulations! You unscrambled the word.";
        } else {
          this.resultMessage = "Oops! Incorrect answer. Try again.";
        }
        this.gameOver = true;
      },
      restartGame() {
        this.generateWord();
        this.userInput = [];
        this.resultMessage = "";
        this.gameOver = false;
      },
      changeCategory() {
        this.restartGame();
      }
    }
  };
  </script>
  
  <style scoped>
  #app {
    text-align: center;
    background-color: #f7f7f7;
    padding: 20px;
  }
  
  .title {
    font-size: 48px;
    margin-bottom: 30px;
    margin-top: 50px; 
    color: #f7f7f7;
    display: inline-block;
    background-color: #6f353a;
    padding: 20px 30px;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease;
  }
  
  .title:hover {
    transform: scale(1.1);
  }
  
  .category-select {
    margin-bottom: 30px;
    margin-top: 30px; 
  }
  
  label {
    font-size: 24px;
    margin-right: 10px;
    color: #333;
  }
  
  select {
    padding: 10px;
    font-size: 20px;
    border-radius: 4px;
  }
  
  p {
    font-size: 24px;
    margin-bottom: 20px;
    margin-top: 50px;
    color: #333;
  }
  
  .instruction {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
    font-weight: bold;
  }
  
  .word {
    font-size: 72px;
    margin-bottom: 40px;
    color: #d87008;
    text-transform: uppercase;
    letter-spacing: 10px;
  }
  
  .word-letter {
    display: inline-block;
    border: 2px solid #ccc;
    padding: 10px;
    margin: 5px;
    background-color: #f7f7f7;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
  }
  
  .word-letter:hover {
    transform: scale(1.1);
    cursor: pointer;
  }
  
  .user-input {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }
  
  .letter {
    font-size: 24px;
    margin: 0 5px;
    padding: 10px;
    background-color: #f7f7f7;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  input {
    padding: 10px;
    margin-right: 10px;
    font-size: 20px;
    border-radius: 4px;
    border: 1px solid #ccc;
  }
  
  button {
    padding: 12px 24px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 24px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  button:disabled {
    background-color: #ccc;
    color: #999;
    cursor: not-allowed;
  }
  
  .result-message {
    font-size: 24px;
    font-weight: bold;
    margin-top: 40px;
    color: #333;
  }
  </style>