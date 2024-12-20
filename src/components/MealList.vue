<template>
  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <MealCard
      v-for="(meal, index) in meals"
      :key="index"
      :meal="meal"
      @delete-meal="() => $emit('delete-meal', index)"
      @update-meal="(updatedMeal) => $emit('update-meal', index, updatedMeal)"
    />
  </div>
</template>

<script setup>
import MealCard from "./MealCard.vue";

defineProps({
  meals: Array,
});
</script>
