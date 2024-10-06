<template>
  <div id="app" :style="{ backgroundColor: backgroundColor }">
    <PomodoroTimer @changeColor="updateBackgroundColor" />
    <footer>
      <div class="social-icons">
        <a
          href="https://github.com/safwan3528"
          target="_blank"
          rel="noopener noreferrer"
        >
          <font-awesome-icon :icon="['fab', 'github']" />
        </a>
        <a
          href="https://www.linkedin.com/in/safwanrahimi"
          target="_blank"
          rel="noopener noreferrer"
        >
          <font-awesome-icon :icon="['fab', 'linkedin']" />
        </a>
        <a
          href="https://safwanrahimi.vercel.app/"
          target="_blank"
          rel="noopener noreferrer"
        >
          <font-awesome-icon :icon="['fas', 'flask']" />
        </a>
      </div>
      <div class="copyright">
        © {{ new Date().getFullYear() }} All rights reserved. Safwan Rahimi
      </div>
    </footer>
  </div>
</template>

<script>
import PomodoroTimer from "./components/PomodoroTimer.vue";
import { library } from "@fortawesome/fontawesome-svg-core";
import { faGithub, faLinkedin } from "@fortawesome/free-brands-svg-icons";
import { faFlask } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(faGithub, faLinkedin, faFlask);

export default {
  name: "App",
  components: {
    PomodoroTimer,
    FontAwesomeIcon,
  },
  data() {
    return {
      backgroundColor: "#2c3e50",
    };
  },
  methods: {
    updateBackgroundColor(color) {
      this.backgroundColor = color;
      document.body.style.backgroundColor = color;
    },
  },
  mounted() {
    document.body.style.backgroundColor = this.backgroundColor;
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

footer {
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.social-icons {
  margin-bottom: 10px;
}

.social-icons a {
  color: white;
  margin: 0 10px;
  font-size: 24px;
  transition: color 0.3s;
}

.social-icons a:hover {
  color: #3498db;
}

.copyright {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.7);
}
</style>
