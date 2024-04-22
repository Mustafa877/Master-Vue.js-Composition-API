<template>
  <div class="centered-form">
    <div class="quote-generator">
      <h1 class="app-title">Inspirational Quotes</h1>

      <div v-if="!isLoading && !error" class="quote-container">
        <p class="quote-text">{{ currentQuote.text }}</p>
        <p class="quote-author">— {{ currentQuote.author }}</p>
      </div>

      <div v-if="isLoading" class="loading">
        <div class="loading-spinner"></div>
        <p>Loading...</p>
      </div>

      <div v-if="error" class="error">{{ error }}</div>

      <button @click="fetchRandomQuote" class="quote-button">
        {{ isLoading ? 'Loading...' : 'Inspire Me' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const currentQuote = ref({ text: "", author: "" });
const isLoading = ref(false);
const error = ref(null);

const getRandomQuote = async () => {
  isLoading.value = true;
  error.value = null;
  try {
    const response = await fetch("https://type.fit/api/quotes");
    const data = await response.json();
    const randomIndex = Math.floor(Math.random() * data.length);
    currentQuote.value = data[randomIndex];
  } catch (error) {
    console.error("Error fetching quotes:", error);
    error.value = "Failed to fetch a quote. Please try again later.";
  } finally {
    isLoading.value = false;
  }
};

const fetchRandomQuote = async () => {
  isLoading.value = true;
  await getRandomQuote();
  isLoading.value = false;
};

fetchRandomQuote(); // Load a quote initially
</script>

<style scoped>
.centered-form {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.quote-generator {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f8f8f8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.app-title {
  font-family: "Pacifico", cursive;
  font-size: 36px;
  margin-bottom: 20px;
  color: #4caf50;
}

.quote-container {
  margin-bottom: 20px;
}

.quote-text {
  font-family: "Kaushan Script", cursive;
  font-size: 24px;
  line-height: 1.6;
  color: #333;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  transition: color 0.3s, transform 0.3s;
}

.quote-text:hover {
  color: #af4c4c;
  transform: scale(1.05);
}
.quote-author:hover {
  color: #af4c4c;
  transform: scale(1.05);
}

.quote-author {
  font-family: "Montserrat", sans-serif;
  font-size: 18px;
  color: #555;
}

.loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 18px;
  margin-bottom: 20px;
}

.loading-spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
  margin-bottom: 10px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error {
  font-size: 18px;
  color: #ff0000;
  margin-bottom: 20px;
}

.quote-button {
  padding: 10px 20px;
  font-size: 18px;
  font-family: "Montserrat", sans-serif;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.quote-button:hover {
  background-color: #45a049;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
