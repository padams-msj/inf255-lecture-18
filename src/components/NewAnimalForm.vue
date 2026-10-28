<script setup>
import { ref } from "vue";

const newAnimal = ref({
  name: "",
  species: "Dog",
  age: 0,
  adopted: false,
});

const emit = defineEmits(["add"]);

function submit() {
  emit("add", newAnimal.value);
  newAnimal.value = { name: "", species: "Dog", age: 0, adopted: false };
}
</script>

<template>
  <form @submit.prevent="submit">
    <label>Name <input v-model="newAnimal.name" required /></label>

    <label>Species
      <select v-model="newAnimal.species">
        <option>Dog</option>
        <option>Cat</option>
        <option>Rabbit</option>
        <option>Guinea pig</option>
      </select>
    </label>

    <label>Age <input type="number" v-model.number="newAnimal.age" min="0" /></label>

    <label><input type="checkbox" v-model="newAnimal.adopted" /> Adopted</label>

    <button>Add</button>
  </form>
</template>
