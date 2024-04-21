
<script setup>
import { ref } from "vue";
const currentQuote = ref({ text: "", author: "" });

const getRandomQuote = async () => {
  try {
    const response = await fetch("https://type.fit/api/quotes");
    const data = await response.json();
    const randomIndex = Math.floor(Math.random() * data.length);
    currentQuote.value = data[randomIndex];
  } catch (error) {
    console.error("Error fetching quotes:", error);
  }
};

getRandomQuote();
</script>

<template>
  <div class="centered-form">
    <div class="quote-generator">
      <h1 class="app-title">Random Quote Generator</h1>

      <blockquote class="quote-container">
        <p>{{ currentQuote.text }}</p>
        <cite>{{ currentQuote.author }}</cite>
      </blockquote>

      <button @click="getRandomQuote" class="quote-button">
        Get Random Quote
      </button>
    </div>
  </div>
</template>



<style scoped>
.centered-form {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.quote-generator {
  max-width: 100%;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.app-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

.quote-container {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #ddd;
  margin-bottom: 20px;
}

p {
  font-size: 18px;
  margin-bottom: 10px;
}

cite {
  font-style: normal;
  color: #777;
}

.quote-button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Add transition */
}

 
.quote-button:hover {
  background-color: #45a049;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
