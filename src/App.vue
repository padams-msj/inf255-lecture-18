<script setup>
import { ref, computed } from "vue";
import AppHeader from "./components/AppHeader.vue";
import AnimalCard from "./components/AnimalCard.vue";
import NewAnimalForm from "./components/NewAnimalForm.vue";

const animals = ref([]);
const loading = ref(false);
const error = ref("");
const search = ref("");

const adoptedCount = computed(() => {
  const adoptedAnimals = animals.value.filter(function (animal) {
    return animal.adopted;
  });
  return adoptedAnimals.length;
});

const filteredAnimals = computed(() => {
  return animals.value.filter((animal) =>
    animal.name.toLowerCase().includes(search.value.toLowerCase())
  );
});

async function loadAnimals() {
  loading.value = true;
  error.value = "";
  try {
    const response = await fetch("/animals.json");
    if (!response.ok) {
      throw new Error("Request failed: " + response.status);
    }
    animals.value = await response.json();
  } catch (err) {
    error.value = "Couldn't load animals. Please try again.";
    console.log(err);
  }
  loading.value = false;
}

loadAnimals();

function toggleAdopted(animal) {
  animal.adopted = !animal.adopted;
}

function addAnimal(animal) {
  animal.id = Date.now();
  animals.value.push(animal);
}
</script>

<template>
  <AppHeader :total="animals.length" :adopted="adoptedCount" />
  <NewAnimalForm @add="addAnimal" />

  <input v-model="search" placeholder="Search by name" />
  <button @click="loadAnimals" :disabled="loading">Reload</button>

  <p v-if="loading">Loading...</p>
  <p v-else-if="error" class="error">{{ error }}</p>
  <p v-else-if="animals.length === 0">No animals found.</p>
  <div v-else class="grid">
    <AnimalCard
      v-for="animal in filteredAnimals"
      :key="animal.id"
      :animal-data="animal"
      @toggle="toggleAdopted(animal)"
    />
  </div>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}
</style>
