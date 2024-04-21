<script setup>
import { ref } from "vue";
import axios from "axios";

const joke = ref(null);
const loading = ref(false);

const fetchJoke = async () => {
  loading.value = true;
  try {
    const response = await axios.get("https://icanhazdadjoke.com/", {
      headers: { Accept: "application/json" },
    });
    joke.value = response.data.joke;
  } catch (error) {
    console.error("Error fetching dad joke:", error);
  } finally {
    loading.value = false;
  }
};
</script>


<template>
  <div class="dad-jokes-container">
    <h2 class="dad-jokes-title">Dad Jokes</h2>
    <button @click="fetchJoke" :disabled="loading" class="get-joke-button">
      <span v-if="loading">Loading...</span>
      <span v-else>Get Dad Joke</span>
    </button>
    <transition name="wave" mode="out-in">
      <div v-if="joke" class="dad-joke" :key="joke">
        {{ joke }}
      </div>
    </transition>
  </div>
</template>



<style scoped>
.dad-jokes-container {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
}

.dad-jokes-title {
  font-size: 36px;
  color: #333;
  margin-bottom: 30px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.get-joke-button {
  padding: 15px 30px;
  font-size: 18px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 40px;
  cursor: pointer;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.2s;
}

.get-joke-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.get-joke-button:hover {
  background-color: #45a049;
  transform: translateY(-2px);
  box-shadow: 0px 6px 8px rgba(0, 0, 0, 0.2);
}

.dad-joke {
  border-radius: 10px;
  padding: 20px;
  margin-top: 30px;
  background: linear-gradient(to bottom, #ffffff 0%, #f8f8f8 100%);
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s;
}

.dad-joke:hover {
  box-shadow: 0px 8px 20px rgba(0, 0, 0, 0.2);
}

.wave-enter-active {
  animation: waveEnter 0.5s ease;
}

@keyframes waveEnter {
  0% {
    transform: scale(0);
    opacity: 0;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.8;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
