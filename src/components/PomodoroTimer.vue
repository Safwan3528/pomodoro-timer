<template>
  <div class="pomodoro-timer">
    <div class="timer-modes">
      <button
        :class="{ active: currentMode === 'pomodoro' }"
        @click="setMode('pomodoro')"
      >
        pomodoro
      </button>
      <button
        :class="{ active: currentMode === 'short break' }"
        @click="setMode('short break')"
      >
        short break
      </button>
      <button
        :class="{ active: currentMode === 'long break' }"
        @click="setMode('long break')"
      >
        long break
      </button>
    </div>
    <div class="timer">{{ formatTime(timeLeft) }}</div>
    <div class="controls">
      <button class="start-button" @click="toggleTimer">
        {{ isRunning ? "PAUSE" : "START" }}
      </button>
      <button class="reset-button" @click="resetTimer">
        <span class="icon">↻</span>
      </button>
      <button class="settings-button" @click="toggleSettings">
        <span class="icon">⚙</span>
      </button>
    </div>
    <div v-if="showSettings" class="settings-panel">
      <div class="time-settings">
        <div v-for="(time, mode) in modes" :key="mode" class="time-setting">
          <label>{{ mode }}:</label>
          <input
            type="number"
            v-model.number="modes[mode]"
            @change="updateTimer"
            min="1"
            max="60"
          />
        </div>
      </div>
      <div class="color-picker">
        <button
          v-for="color in colors"
          :key="color"
          :style="{ backgroundColor: color }"
          @click="selectColor(color)"
          class="color-option"
        ></button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeLeft: 25 * 60,
      isRunning: false,
      timer: null,
      currentMode: "pomodoro",
      modes: {
        pomodoro: 30,
        "short break": 5,
        "long break": 15,
      },
      showSettings: false,
      colors: [
        "#000000",
        "#1a1a1a",
        "#2c3e50",
        "#34495e",
        "#3498db",
        "#2ecc71",
        "#e74c3c",
        "#9b59b6",
      ],
      audio: new Audio("/timer-end.mp3"),
    };
  },
  methods: {
    toggleTimer() {
      if (this.isRunning) {
        this.pauseTimer();
      } else {
        this.startTimer();
      }
    },
    startTimer() {
      this.isRunning = true;
      this.timer = setInterval(() => {
        if (this.timeLeft > 0) {
          this.timeLeft--;
        } else {
          this.pauseTimer();
          this.playEndSound(); // Tambahkan ini
        }
      }, 1000);
    },
    pauseTimer() {
      clearInterval(this.timer);
      this.isRunning = false;
    },
    resetTimer() {
      this.pauseTimer();
      this.timeLeft = this.modes[this.currentMode] * 60;
    },
    setMode(mode) {
      this.currentMode = mode;
      this.timeLeft = this.modes[mode] * 60;
      this.pauseTimer();
    },
    formatTime(seconds) {
      const mins = Math.floor(seconds / 60);
      const secs = seconds % 60;
      return `${mins.toString().padStart(2, "0")}:${secs
        .toString()
        .padStart(2, "0")}`;
    },
    toggleSettings() {
      this.showSettings = !this.showSettings;
    },
    selectColor(color) {
      this.$emit("changeColor", color);
    },
    updateTimer() {
      this.timeLeft = this.modes[this.currentMode] * 60;
    },
    playEndSound() {
      // Tambahkan metode ini
      this.audio.play();
    },
  },
};
</script>

<style scoped>
.pomodoro-timer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  width: 100%;
  flex: 1;
  padding: 20px;
  box-sizing: border-box;
  overflow-y: auto;
}

.timer-modes {
  display: flex;
  margin-bottom: 20px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 30px;
  padding: 5px;
}

.timer-modes button {
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.6);
  padding: 8px 16px;
  cursor: pointer;
  border-radius: 25px;
  transition: background-color 0.3s, color 0.3s;
  font-size: 16px;
}

.timer-modes button.active {
  background-color: rgba(255, 255, 255, 0.2);
  color: white;
}

.timer {
  font-size: 100px;
  font-weight: bold;
  margin: 20px 0;
}

.controls {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.start-button {
  background-color: white;
  color: #2c3e50;
  border: none;
  padding: 12px 40px;
  font-size: 20px;
  border-radius: 30px;
  cursor: pointer;
  font-weight: bold;
  margin-right: 15px;
  text-transform: uppercase;
}

.reset-button,
.settings-button {
  background: none;
  border: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
  margin: 0 8px;
}

.icon {
  font-size: 24px;
}

.settings-panel {
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 15px;
  margin-top: 15px;
}

.time-settings {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}

.time-setting {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.time-setting label {
  margin-bottom: 5px;
  font-size: 14px;
}

.time-setting input {
  width: 50px;
  padding: 6px;
  border: none;
  border-radius: 5px;
  text-align: center;
  font-size: 14px;
}

.color-picker {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

.color-option {
  width: 30px;
  height: 30px;
  border: none;
  border-radius: 50%;
  margin: 0 6px;
  cursor: pointer;
  transition: transform 0.2s;
}

.color-option:hover {
  transform: scale(1.1);
}
</style>
