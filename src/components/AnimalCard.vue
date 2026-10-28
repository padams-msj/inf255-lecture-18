<script setup>
defineProps({
  animalData: Object,
});

const emit = defineEmits(["toggle"]);
</script>

<template>
  <div class="card" :class="{ adopted: animalData.adopted }">
    <h2>{{ animalData.name }}</h2>
    <p>{{ animalData.species }}, {{ animalData.age }} years old</p>
    <p>{{ animalData.adopted ? "Adopted" : "Available" }}</p>
    <button @click="emit('toggle')">Toggle</button>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 1rem;
}

.card.adopted {
  opacity: 0.5;
}
</style>
