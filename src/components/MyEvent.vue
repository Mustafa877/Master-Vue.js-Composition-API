<script setup>
const ShowMessage = (message) => alert(message)
</script>

<template>
  <button @click="ShowMessage('this my msg')">Increment</button>
</template>
