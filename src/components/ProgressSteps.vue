
<script setup>
import { ref } from 'vue'

const steps = ref(['Step 1', 'Step 2', 'Step 3', 'Step 4'])
const currentStep = ref(0)

const nextStep = () => {
  if (currentStep.value < steps.value.length - 1) {
    currentStep.value++
  }
}

const prevStep = () => {
  if (currentStep.value > 0) {
    currentStep.value--
  }
}

const goToStep = (stepIndex) => {
  currentStep.value = stepIndex
}
</script>

<template>
  <div class="progress-container">
    <div class="progress-bar">
      <div
        v-for="(step, index) in steps"
        :key="index"
        :class="{ 'step-active': index === currentStep }"
        class="step"
        @click="goToStep(index)"
      >
        {{ step }}
      </div>
    </div>
    <div class="controls">
      <button @click="prevStep" :disabled="currentStep === 0" class="btn">Previous</button>
      <button @click="nextStep" :disabled="currentStep === steps.length - 1" class="btn">Next</button>
    </div>
  </div>
</template>



<style scoped>
.progress-container {
  max-width: 500px;
  margin: 50px auto;
}

.progress-bar {
  display: flex;
  background-color: #f0f0f0;
  border-radius: 8px;
  overflow: hidden;
}

.step {
  flex: 1;
  text-align: center;
  padding: 15px;
  color: #555;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.step:hover {
  background-color: #f5f5f5;
}

.step-active {
  background-color: #5e72e4;
  color: #fff;
  font-weight: bold;
}

.controls {
  margin-top: 20px;
  text-align: center;
}

.btn {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #5e72e4;
  color: #fff;
  border: none;
  border-radius: 5px;
  margin: 0 5px;
  transition: background-color 0.3s ease;
}

.btn:hover {
  background-color: #324cdd;
}

.btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
