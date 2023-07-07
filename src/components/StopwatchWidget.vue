<template>
    <div class="stopwatch-widget">
      <div class="clock">
        <div class="time-box time-box-hours">{{ formatHours }}</div>
        <div class="time-separator">:</div>
        <div class="time-box time-box-minutes">{{ formatMinutes }}</div>
        <div class="time-separator">:</div>
        <div class="time-box time-box-seconds">{{ formatSeconds }}</div>
        <div class="time-separator">:</div>
        <div class="time-box time-box-milliseconds">{{ formatMilliseconds }}</div>
      </div>
      <div class="button-group">
        <button v-if="!isRunning" @click="startStopwatch">Start</button>
        <button v-if="isRunning && !isPaused" @click="pauseStopwatch">Pause</button>
        <button v-if="isPaused" @click="resetStopwatch">Reset</button>
        <button v-if="isPaused" @click="resumeStopwatch">Resume</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isRunning: false,
        isPaused: false,
        startTime: null,
        elapsedTime: 0,
      };
    },
    computed: {
      formatHours() {
        return Math.floor(this.elapsedTime / 3600000)
          .toString()
          .padStart(2, '0');
      },
      formatMinutes() {
        return Math.floor((this.elapsedTime / 60000) % 60)
          .toString()
          .padStart(2, '0');
      },
      formatSeconds() {
        return Math.floor((this.elapsedTime / 1000) % 60)
          .toString()
          .padStart(2, '0');
      },
      formatMilliseconds() {
        return Math.floor((this.elapsedTime / 10) % 100)
          .toString()
          .padStart(2, '0');
      },
    },
    methods: {
      startStopwatch() {
        this.isRunning = true;
        this.startTime = Date.now();
  
        this.timerInterval = setInterval(() => {
          this.elapsedTime = Date.now() - this.startTime;
        }, 10);
      },
      pauseStopwatch() {
        this.isPaused = true;
        clearInterval(this.timerInterval);
      },
      resetStopwatch() {
        this.isPaused = false;
        this.isRunning = false;
        clearInterval(this.timerInterval);
        this.elapsedTime = 0;
      },
      resumeStopwatch() {
        this.isPaused = false;
        this.startTime = Date.now() - this.elapsedTime;
  
        this.timerInterval = setInterval(() => {
          this.elapsedTime = Date.now() - this.startTime;
        }, 10);
      },
    },
  };
  </script>
  
  <style scoped>
  .stopwatch-widget {
    border: 1px solid #ccc;
    padding: 20px;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .stopwatch-widget .clock {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .stopwatch-widget .time-box {
    width: 60px;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    font-size: 24px;
    background-color: #d87008;
    color: white;
  }
  
  .stopwatch-widget .time-separator {
    font-size: 36px; 
    color: white;
    margin: 0 6px;
  }
  
  .stopwatch-widget .button-group {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }
  
  .stopwatch-widget button {
    padding: 10px 20px;
    background-color: #66bb6a;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    margin: 0 5px;
  }
  
  .stopwatch-widget button:hover {
    background-color: #81c784;
  }
  
  .stopwatch-widget button:disabled {
    background-color: #ccc;
    color: #999;
    cursor: not-allowed;
  }
  </style>