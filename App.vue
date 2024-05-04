<template>
  <div>
    <h2>Class Binding, Style Binding, dan Event Listener</h2>

    <div 
      :class="{ 'alert': showAlert, 'success': showSuccess }"
      :style="{ 'font-size': fontSize + 'px', 'color': fontColor }"
      @mouseover="changeFontSize"
      @mouseleave="resetFontSize"
    >
      {{ message }}
    </div>

    <button @click="toggleAlert">Toggle Alert</button>
    <button @click="toggleSuccess">Toggle Success</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const showAlert = ref(false)
const showSuccess = ref(false)
const message = ref("Ini adalah pesan")
const fontSize = ref(16)
const fontColor = ref("black")

function toggleAlert() {
  showAlert.value = !showAlert.value
}

function toggleSuccess() {
  showSuccess.value = !showSuccess.value
}

function changeFontSize() {
  fontSize.value = 20
  fontColor.value = "blue"
}

function resetFontSize() {
  fontSize.value = 16
  fontColor.value = "black"
}
</script>

<style scoped>
.alert {
  background-color: #f44336; /* Merah */
  color: white;
  padding: 10px;
}

.success {
  background-color: #4CAF50; /* Hijau */
  color: white;
  padding: 10px;
}
</style>
