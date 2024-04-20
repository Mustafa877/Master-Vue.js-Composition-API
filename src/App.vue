<script setup>
import { ref } from "vue";

const passwordLength = ref(12);
const includeUppercase = ref(true);
const includeNumbers = ref(true);
const includeSymbols = ref(true);
const generatedPassword = ref("");
const copiedToClipboard = ref(false);

const generatePassword = () => {
  const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
  const uppercaseChars = includeUppercase.value
    ? "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    : "";
  const numberChars = includeNumbers.value ? "0123456789" : "";
  const symbolChars = includeSymbols.value ? "!@#$%^&*()_+[]{}|;:,.<>?/~`" : "";

  const allChars = lowercaseChars + uppercaseChars + numberChars + symbolChars;

  let password = "";
  for (let i = 0; i < passwordLength.value; i++) {
    const randomIndex = Math.floor(Math.random() * allChars.length);
    password += allChars[randomIndex];
  }

  generatedPassword.value = password;
};

const copyToClipboard = () => {
  navigator.clipboard.writeText(generatedPassword.value);
  copiedToClipboard.value = true;
  setTimeout(() => {
    copiedToClipboard.value = false;
  }, 2000); // Reset copiedToClipboard after 2 seconds
};
</script>



<template>
  <div class="password-generator-container">
    <h2 class="password-generator-title">Password Generator</h2>
    <div class="input-container">
      <label for="length">Password Length:</label>
      <input
        type="number"
        id="length"
        v-model="passwordLength"
        min="4"
        max="32"
        class="input-field"
      />
    </div>
    <div class="checkbox-container">
      <label>
        <input type="checkbox" v-model="includeUppercase" />
        Include Uppercase
      </label>
      <label>
        <input type="checkbox" v-model="includeNumbers" />
        Include Numbers
      </label>
      <label>
        <input type="checkbox" v-model="includeSymbols" />
        Include Symbols
      </label>
    </div>
    <button @click="generatePassword" class="generate-button">
      Generate Password
    </button>
    <div v-if="generatedPassword" class="generated-password">
      <strong>Your Password:</strong> {{ generatedPassword }}
      <button @click="copyToClipboard" class="copy-button">Copy</button>
    </div>
    <div v-if="copiedToClipboard" class="copied-message">Password copied to clipboard!</div>
  </div>
</template>



<style scoped>
.password-generator-container {
  max-width: 400px;
  margin: 50px auto;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.password-generator-title {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

.input-container {
  margin-bottom: 20px;
}

.input-field {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.checkbox-container {
  margin-bottom: 20px;
}

.checkbox-container label {
  display: block;
  margin-bottom: 8px;
}

.generate-button {
  padding: 10px 15px;
  font-size: 16px;
  background-color: #3498db;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.generate-button:hover {
  background-color: #2980b9;
}

.generated-password {
  margin-top: 20px;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #fff;
  color: #333;
  display: flex;
  justify-content: space-between;
}

.copy-button {
  padding: 8px 12px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.copy-button:hover {
  background-color: #45a049;
}

.copied-message {
  background-color: #4caf50;
  color: #fff;
  padding: 8px;
  border-radius: 4px;
  text-align: center;
  margin-top: 10px;
}
</style>
