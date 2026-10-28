<script setup>
defineProps({
  total: Number,
  adopted: Number,
});
</script>

<template>
  <header>
    <h1>Animal Shelter</h1>
    <p>{{ adopted }} of {{ total }} adopted</p>
  </header>
</template>
