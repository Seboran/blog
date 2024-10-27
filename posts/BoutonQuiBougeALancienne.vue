<template>
  <button class="button" type="button" @click="alerteAMalibu">
    bouton alerte à malibu !!!
  </button>
</template>

<script setup lang="ts">
function alerteAMalibu() {
  alert('malibu')
}
</script>

<style lang="css" scoped>
.button {
  background-color: red;
  border-radius: 2rem;
  padding: 0.5rem;
  color: white;
}

.button:hover {
  background-color: brown;
}

.button:active {
  background-color: green;
}
</style>
