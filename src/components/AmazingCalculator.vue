<script setup>
import { ref, computed } from "vue";

const display = ref("0");

const appendToDisplay = (value) => {
  if (display.value === "0" && value !== ".") {
    display.value = value;
  } else {
    display.value += value;
  }
};

const calculate = () => {
  try {
    display.value = eval(display.value).toString();
  } catch (error) {
    display.value = "Error";
  }
};

// Computed property for dynamic class binding
const displayClass = computed(() => {
  return display.value.length > 12 ? "small-text" : "";
});

const clearDisplay = () => {
  display.value = "0";
};

const handleKeydown = (event) => {
  const key = event.key;
  if (!isNaN(key) || key === "." || "+-*/".includes(key)) {
    appendToDisplay(key);
  } else if (key === "Enter") {
    calculate();
  } else if (key === "Escape") {
    clearDisplay();
  }
};
</script>

<template>
  <div class="centered-form">
  <div class="calculator">
    <input v-model="display" :class="displayClass" readonly @keydown="handleKeydown" />

    <div class="buttons">
      <button @click="appendToDisplay('7')">7</button>
      <button @click="appendToDisplay('8')">8</button>
      <button @click="appendToDisplay('9')">9</button>
      <button @click="appendToDisplay('/')">÷</button>

      <button @click="appendToDisplay('4')">4</button>
      <button @click="appendToDisplay('5')">5</button>
      <button @click="appendToDisplay('6')">6</button>
      <button @click="appendToDisplay('*')">×</button>

      <button @click="appendToDisplay('1')">1</button>
      <button @click="appendToDisplay('2')">2</button>
      <button @click="appendToDisplay('3')">3</button>
      <button @click="appendToDisplay('-')">−</button>

      <button @click="appendToDisplay('0')">0</button>
      <button @click="appendToDisplay('.')">.</button>
      <button @click="calculate()">=</button>
      <button @click="appendToDisplay('+')">+</button>
    </div>

    <button @click="clearDisplay" class="clear-button">C</button>
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
.calculator {
  max-width: 300px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

input {
  width: 90%;
  margin-bottom: 20px;
  padding: 15px;
  font-size: 24px;
  text-align: right;
  border: none;
  border-radius: 5px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  width: 100%;
  padding: 15px;
  font-size: 24px;
  border: none;
  border-radius: 5px;
  background-color: #e0e0e0;
  color: #333;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #ccc;
}

.clear-button {
  width: 100%;
  padding: 15px;
  font-size: 20px;
  margin-top: 10px;
  border: none;
  border-radius: 5px;
  background-color: #ff6347;
  color: #fff;
  transition: background-color 0.3s ease;
}

.clear-button:hover {
  background-color: #ff483f;
}

.small-text {
  font-size: 18px;
}

@media screen and (max-width: 480px) {
  .buttons {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
